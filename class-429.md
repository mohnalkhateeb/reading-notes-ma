# Room
## Save data in a local database using Room   
* #### Setup
        dependencies {
            def room_version = "2.3.0"

            implementation "androidx.room:room-runtime:$room_version"
            annotationProcessor "androidx.room:room-compiler:$room_version"

            // optional - RxJava2 support for Room
            implementation "androidx.room:room-rxjava2:$room_version"

            // optional - RxJava3 support for Room
            implementation "androidx.room:room-rxjava3:$room_version"

            // optional - Guava support for Room, including Optional and ListenableFuture
            implementation "androidx.room:room-guava:$room_version"

            // optional - Test helpers
            testImplementation "androidx.room:room-testing:$room_version"

            // optional - Paging 3 Integration
            implementation "androidx.room:room-paging:2.4.0-beta01"
        }

* ### Primary components
![pc](https://developer.android.com/images/training/data-storage/room_architecture.png)

* ### Sample implementation
    - ##### Data entity
            @Entity
            public class User {
            @PrimaryKey
            public int uid;

            @ColumnInfo(name = "first_name")
            public String firstName;

            @ColumnInfo(name = "last_name")
            public String lastName;
        }
    
    - ##### Data access object (DAO)
                 @Dao
                public interface UserDao {
                @Query("SELECT * FROM user")
                List<User> getAll();

                @Query("SELECT * FROM user WHERE uid IN (:userIds)")
                List<User> loadAllByIds(int[] userIds);

                @Query("SELECT * FROM user WHERE first_name LIKE :first AND " +
                    "last_name LIKE :last LIMIT 1")
                User findByName(String first, String last);

                @Insert
                void insertAll(User... users);

                @Delete
                void delete(User user);
            }

    - ##### Database
            @Database(entities = {User.class}, version = 1)
            public abstract class AppDatabase extends RoomDatabase {
            public abstract UserDao userDao();
            }

## Defining data using Room entities
* #### Anatomy of an entity
    - each Room entity as a class that is annotated with @Entity

            @Entity
            public class User {
            @PrimaryKey
            public int id;

            public String firstName;
            public String lastName;
        }

* #### Define a primary key
        @PrimaryKey
        public int id;

* #### Define a composite primary key
        @Entity(primaryKeys = {"firstName", "lastName"})
        public class User {
            public String firstName;
            public String lastName;
        }
* #### Ignore fields
        @Entity
        public class User {
            @PrimaryKey
            public int id;

            public String firstName;
            public String lastName;

            @Ignore
            Bitmap picture;
        }
or this 

             @Entity(ignoredColumns = "picture")
            public class RemoteUser extends User {
                @PrimaryKey
                public int id;

                public boolean hasVpn;
            }

* ### Provide table search support
    - ##### Support full-text search
            @Fts4(languageId = "lid")
            @Entity(tableName = "users")
            public class User {
                // ...

            @ColumnInfo(name = "lid")
            int languageId;
            }

    - ##### Index specific columns
                @Entity(indices = {@Index(value = {"first_name", "last_name"},
                    unique = true)})
                public class User {
                @PrimaryKey
                public int id;

                @ColumnInfo(name = "first_name")
                public String firstName;

                @ColumnInfo(name = "last_name")
                public String lastName;

                @Ignore
                Bitmap picture;
            }
* #### Include AutoValue-based objects
        @AutoValue
        @Entity
        public abstract class User {
            // Supported annotations must include `@CopyAnnotations`.
            @CopyAnnotations
            @PrimaryKey
            public abstract long getId();

            public abstract String getFirstName();
            public abstract String getLastName();

            // Room uses this factory method to create User objects.
            public static User create(long id, String firstName, String lastName) {
                return new AutoValue_User(id, firstName, lastName);
            }
        }

## Define relationships between objects 
* #### Using Intermediate data class
        @Dao
        public interface UserBookDao {
        @Query("SELECT user.name AS userName, book.name AS bookName " +
                "FROM user, book " +
                "WHERE user.id = book.user_id")
        public LiveData<List<UserBook>> loadUserAndBookNames();
        }

        public class UserBook {
            public String userName;
            public String bookName;
        }
* #### Multimap return types
        @Query(
            "SELECT * FROM user" +
            "JOIN book ON user.id = book.user_id"
        )
        public Map<User, List<Book>> loadUserAndBookNames();

* ### Create embedded objects
    object that you'd like to decompose into its subfields within a table. You can then query the embedded fields just as you would for other individual columns.

        public class Address {
            public String street;
            public String state;
            public String city;

            @ColumnInfo(name = "post_code") public int postCode;
        }

        @Entity
        public class User {
            @PrimaryKey public int id;

            public String firstName;

            @Embedded public Address address;
        }

* ### Define one-to-one relationships
    - First, create a class for each of your two entities. One of the entities must include a variable that is a reference to the primary key of the other entity.

            @Entity
            public class User {
                @PrimaryKey public long userId;
                public String name;
                public int age;
            }
            @Entity
            public class Library {
                @PrimaryKey public long libraryId;
                public long userOwnerId;
            }

    - Add the `@Relation` annotation to the instance of the child entity, with `parentColumn` set to the name of the primary key column of the parent entity and `entityColumn` set to the name of the column of the child entity that references the parent entity's primary key.

            public class UserAndLibrary {
            @Embedded public User user;
            @Relation(
                parentColumn = "userId",
                entityColumn = "userOwnerId"
            )
            public Library library;
        }

    - Finally, add a method to the DAO class that returns all instances of the data class that pairs the parent entity and the child entity. This method requires Room to run two queries, so add the @Transaction annotation to this method to ensure that the whole operation is performed atomically.

                @Transaction
            @Query("SELECT * FROM User")
            public List<UserAndLibrary> getUsersAndLibraries();

* #### Define one-to-many relationships
    - First, create a class for each of your two entities. As in the previous example, the child entity must include a variable that is a reference to the primary key of the parent entity.

            @Entity
            public class User {
                @PrimaryKey public long userId;
                public String name;
                public int age;
            }

            @Entity
            public class Playlist {
                @PrimaryKey public long playlistId;
                public long userCreatorId;
                public String playlistName;
            }

    - Add the `@Relation` annotation to the instance of the child entity, with `parentColumn` set to the name of the primary key column of the parent entity and `entityColumn` set to the name of the column of the child entity that references the parent entity's primary key.

                public class UserWithPlaylists {
                @Embedded public User user;
                @Relation(
                    parentColumn = "userId",
                    entityColumn = "userCreatorId"
                )
                public List<Playlist> playlists;
            }

    - Finally, add a method to the DAO class that returns all instances of the data class that pairs the parent entity and the child entity. 

                @Transaction
            @Query("SELECT * FROM User")
            public List<UserWithPlaylists> getUsersWithPlaylists();

* ### Define many-to-many relationships

            public class PlaylistWithSongs {
            @Embedded public Playlist playlist;
            @Relation(
                parentColumn = "playlistId",
                entityColumn = "songId",
                associateBy = @Junction(PlaylistSongCrossref.class)
            )
            public List<Song> songs;
        }

        public class SongWithPlaylists {
            @Embedded public Song song;
            @Relation(
                parentColumn = "songId",
                entityColumn = "playlistId",
                associateBy = @Junction(PlaylistSongCrossref.class)
            )
            public List<Playlist> playlists;
        }

## Accessing data using Room DAOs


### Convenience methods
* #### Insert
        @Dao
        public interface UserDao {
            @Insert(onConflict = OnConflictStrategy.REPLACE)
            public void insertUsers(User... users);

            @Insert
            public void insertBothUsers(User user1, User user2);

            @Insert
            public void insertUsersAndFriends(User user, List<User> friends);
        }
    
* #### Update

        @Dao
        public interface UserDao {
            @Update
            public void updateUsers(User... users);
        }

* #### Delete
        @Dao
        public interface UserDao {
            @Delete
            public void deleteUsers(User... users);
        }

* ### Query methods
    - ##### Simple queries

            @Query("SELECT * FROM user")
            public User[] loadAllUsers();

    - ##### Query multiple tables

                @Query("SELECT * FROM book " +
                "INNER JOIN loan ON loan.book_id = book.id " +
                "INNER JOIN user ON user.id = loan.user_id " +
                "WHERE user.name LIKE :userName")
            public List<Book> findBooksBorrowedByNameSync(String userName);