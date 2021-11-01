# RecyclerView
## Create dynamic lists with RecyclerView
RecyclerView makes it easy to efficiently display large sets of data. You supply the data and define how each item looks, and the RecyclerView library dynamically creates the elements when they're needed.

### Key classes
Several different classes work together to build your dynamic list.

-  `RecyclerView` is the `ViewGroup` that contains the views corresponding to your data. It's a view itself, so you add `RecyclerView` into your layout the way you would add any other UI element.

- Each individual element in the list is defined by a view holder object. When the view holder is created, it doesn't have any data associated with it. After the view holder is created, the `RecyclerView` binds it to its data. You define the view holder by extending `RecyclerView.ViewHolder`.

- The `RecyclerView` requests those views, and binds the views to their data, by calling methods in the adapter. You define the adapter by extending `RecyclerView.Adapter`.

- The layout manager arranges the individual elements in your list. You can use one of the layout managers provided by the RecyclerView library, or you can define your own. Layout managers are all based on the library's `LayoutManager` abstract class

### Steps for implementing your RecyclerView
- First of all, decide what the list or grid is going to look like. Ordinarily you'll be able to use one of the RecyclerView library's standard layout managers.

- Design how each element in the list is going to look and behave. Based on this design, extend the `ViewHolder` class. Your version of `ViewHolder` provides all the functionality for your list items. Your view holder is a wrapper around a `View`, and that `view` is managed by `RecyclerView`.

- Define the `Adapter` that associates your data with the `ViewHolder` views.

### Plan your layout

The items in your RecyclerView are arranged by a `LayoutManager` class. The RecyclerView library provides three layout managers, which handle the most common layout situations:

- `LinearLayoutManager` arranges the items in a one-dimensional list.
- `GridLayoutManager` arranges all items in a two-dimensional grid:
    - If the grid is arranged vertically, `GridLayoutManager` tries to make all the elements in each row have the same width and height, but different rows can have different heights.
    - If the grid is arranged horizontally, `GridLayoutManager` tries to make all the elements in each column have the same width and height, but different columns can have different widths.
- `StaggeredGridLayoutManager` is similar to `GridLayoutManager`, but it does not require that items in a row have the same height (for vertical grids) or items in the same column have the same width (for horizontal grids). The result is that the items in a row or column can end up offset from each other.

### Implementing your adapter and view holder
When you define your adapter, you need to override three key methods:

- `onCreateViewHolder()`: `RecyclerView` calls this method whenever it needs to create a new `ViewHolder`. The method creates and initializes the `ViewHolder` and its associated `View`, but does not fill in the view's contents—the `ViewHolder` has not yet been bound to specific data.

- `onBindViewHolder()`: `RecyclerView` calls this method to associate a `ViewHolder` with data. The method fetches the appropriate data and uses the data to fill in the view holder's layout. For example, if the `RecyclerView` displays a list of names, the method might find the appropriate name in the list and fill in the view holder's `TextView` widget.

- `getItemCount()`: RecyclerView calls this method to get the size of the data set. For example, in an address book app, this might be the total number of addresses. RecyclerView uses this to determine when there are no more items that can be displayed.

* ###### Example 

        public class CustomAdapter extends RecyclerView.Adapter<CustomAdapter.ViewHolder> {

            private String[] localDataSet;

            /**
            * Provide a reference to the type of views that you are using
            * (custom ViewHolder).
            */
            public static class ViewHolder extends RecyclerView.ViewHolder {
                private final TextView textView;

                public ViewHolder(View view) {
                    super(view);
                    // Define click listener for the ViewHolder's View

                    textView = (TextView) view.findViewById(R.id.textView);
                }

                public TextView getTextView() {
                    return textView;
                }
            }

            /**
            * Initialize the dataset of the Adapter.
            *
            * @param dataSet String[] containing the data to populate views to be used
            * by RecyclerView.
            */
            public CustomAdapter(String[] dataSet) {
                localDataSet = dataSet;
            }

            // Create new views (invoked by the layout manager)
            @Override
            public ViewHolder onCreateViewHolder(ViewGroup viewGroup, int viewType) {
                // Create a new view, which defines the UI of the list item
                View view = LayoutInflater.from(viewGroup.getContext())
                        .inflate(R.layout.text_row_item, viewGroup, false);

                return new ViewHolder(view);
            }

            // Replace the contents of a view (invoked by the layout manager)
            @Override
            public void onBindViewHolder(ViewHolder viewHolder, final int position) {

                // Get element from your dataset at this position and replace the
                // contents of the view with that element
                viewHolder.getTextView().setText(localDataSet[position]);
            }

            // Return the size of your dataset (invoked by the layout manager)
            @Override
            public int getItemCount() {
                return localDataSet.length;
            }
        }

* `text_row_item.xml` file like this

        <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:layout_width="match_parent"
            android:layout_height="@dimen/list_item_height"
            android:layout_marginLeft="@dimen/margin_medium"
            android:layout_marginRight="@dimen/margin_medium"
            android:gravity="center_vertical">

            <TextView
                android:id="@+id/textView"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="@string/element_text"/>
        </FrameLayout>