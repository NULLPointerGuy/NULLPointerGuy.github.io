---
layout: post
title: Swipe to dismiss in recyclerview.
---

In order to implement swipe to dismiss in recyclerview we extend a class called **ItemTouchHelper.SimpleCallback** in which we override onMove(just return false) and  **onSwiped** methods.

To get started we need to attach **ItemTouchHelper** class to the recyclerview,

 {% highlight java %}
   
   ItemTouchHelper.Callback callback = new TaskTouchHelper(taskAdapter);
   ItemTouchHelper helper = new ItemTouchHelper(callback);
   helper.attachToRecyclerView(list);

 {% endhighlight %}

I have created a class called **TaskTouchHelper** that extends **ItemTouchHelper.SimpleCallback**, where i pass my adapter class,the main purpose behind this is that when i recieve callback on swipe of recyclerview item that is **onSwiped** method, i get viewholder that is swiped and the direction in which it is swiped,please note that viewholder.getposition()  gives the position of the holder that is swiped. 

Using which i can call adapter.deleteItem(position) which it deletes the item from the adapter.


{% highlight java %}

	 @Override
    public void onSwiped(RecyclerView.ViewHolder viewHolder, 
    	int direction) {
        taskAdapter.deleteItem(viewHolder.getAdapterPosition());
    }

{% endhighlight %}

and my delete method inside the adapter would be implemented in the following way

{% highlight java %}
	
	public void deleteItem(int position){
        db.deleteTask(taskItems.get(position));
        taskItems.remove(position);
        notifyItemRemoved(position);
    }

{% endhighlight %}

and that's it!! 
Recyclerview takes care of all the animations on deletions etc.The source code is available [here](https://github.com/karthikrk24/AndroidTesting.git).<br/> 













