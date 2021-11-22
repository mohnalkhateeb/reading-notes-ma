# Location
![location](https://www.apkmirror.com/wp-content/uploads/2018/04/5ae081ee1d785.png)

## Get the last known location
- Using the Google Play services location APIs, your app can request the last known location of the user's device. In most cases, you are interested in the user's current location, which is usually equivalent to the last known location of the device.

- Specifically, use the fused location provider to retrieve the device's last known location. The fused location provider is one of the location APIs in Google Play services. It manages the underlying location technology and provides a simple API so that you can specify requirements at a high level, like high accuracy or low power. It also optimizes the device's use of battery power.

### Set up Google Play services
- To access the fused location provider, your app's development project must include Google Play services. Download and install the Google Play services component via the SDK Manager and add the library to your project

        apply plugin: 'com.android.application'

        ...

        dependencies {
            implementation 'com.google.android.gms:play-services-location:18.0.0'
        }


### Specify app permissions

        <!-- Recommended for Android 9 (API level 28) and lower. -->
        <!-- Required for Android 10 (API level 29) and higher. -->
        <service
            android:name="MyNavigationService"
            android:foregroundServiceType="location" ... >
            <!-- Any inner elements would go here. -->
        </service>

        <manifest ... >
        <!-- Always include this permission -->
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

        <!-- Include only if your app benefits from precise location access. -->
        <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
        </manifest>

        <manifest ... >
        <!-- Required only when requesting background location access on
            Android 10 (API level 29) and higher. -->
        <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
        </manifest>


### Create location services client

        private FusedLocationProviderClient fusedLocationClient;

        // ..

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            // ...

            fusedLocationClient = LocationServices.getFusedLocationProviderClient(this);
        }

### Get the last known location

        fusedLocationClient.getLastLocation()
                .addOnSuccessListener(this, new OnSuccessListener<Location>() {
                    @Override
                    public void onSuccess(Location location) {
                        // Got last known location. In some rare situations this can be null.
                        if (location != null) {
                            // Logic to handle location object
                        }
                    }
                });