---
title: Writing Classes in Android.
---
**Part 2:**
This blog post discusses principles involved in writing classes(missed in the part 1 post) and on how we can apply them in android.

<!--more-->
 

 >Client code should not be dependent on the class's private implementation.

 The classic example of the above statement in android is whenever we use setText routine in `TextView` we blindly pass `null` as an arguement, we know for a fact that setText internally calls this routine which as a null check.

 {% highlight java %}
   private void setText(CharSequence text, BufferType type,
          boolean notifyBefore, int oldlen) {
        
       if (text == null) {
            text = "";
       }
 {% endhighlight %} 

 tomorrow if android decides to change the implementation of this routine to `throw a nullpointer exception`.
 this might be one of the lame reason why your app might start crashing.
 
 **tl;dr of the principle:** It's good to go through the source code of the textview class in order to have better understanding of it, but it is bad to rely on the `implementation details of the setText routine`.


 >Never reuse the names of the non overrible base class routines. 

as we know `Java` allows us to reuse the name of the non overrible private base class routine in the derived class, client using the class might be confused as it looks polymorphic. major thing to note here is **code is read far more times than written.**

>Prefer Polymorphism to extensive type checking.

Let's say you have class called FeedsAdapter that extends RecyclerView.Adapter, let's assume that you have to use three different types of the ViewHolders.

Now normally in the `onBindViewHolder` routine of the adapter class we would be doing `extensive` type checking and then actually be setting text, images etc, as shown below


{% highlight java %}
    @Override
    public void onBindViewHolder(RecyclerView.ViewHolder viewholder, int position){
          if(viewholder instanceof FeedsHolder){

          }
          else if(viewholder instanceof CommentsHolder){

          }
    } 
{% endhighlight %} 

Please note:
Since onCreateViewHolder routine inflates three different layout `type checking` is required in the routine, hence we are deviating from this principle with a purpose.


We can define a `abstract BaseHolder` class that extends RecyclerView.ViewHolder inside your adapter.  BaseHolder has a abstract routine named `onBind(int position)` , now we can make all the other viewholders extend this class and implement thier own onBind routine as shown below

{% highlight java %}
    

    public abstract class BaseHolder extends RecyclerView.ViewHolder{

        public BaseHolder(View itemView) {
            super(itemView);
        }

        public abstract void onBind(int position);
    }


    public class CommentsHolder extends BaseHolder{
        @BindView(R.id.user_name) TextView user_name;
        @BindView(R.id.user_image) CircleImageView userImage;
        @BindView(R.id.user_comment) TextView user_comment;
        public CommentsHolder(View itemView) {
            super(itemView);
            ButterKnife.bind(this,itemView);
        }

        @Override
        public void onBind(int position) {
            if(postMeta.isDribble()){
                picasso.load(commonDribbleList.get(position).user.avatar_url).into(userImage);
                user_name.setText(commonDribbleList.get(position).user.name);
                user_comment.setText(Html.fromHtml(commonDribbleList.get(position).body));
            }else{
                picasso.load(commonBehanceList[position].user.images.medium).into(userImage);
                user_name.setText(commonBehanceList[position].user.username);
                user_comment.setText(commonBehanceList[position].comment);
            }
        }
    }

    public class CommentsIconHolder extends BaseHolder{
        @BindView(R.id.txt_views) TextView views_count;
        @BindView(R.id.txt_comment) TextView comment_count;
        @BindView(R.id.txt_like) TextView like_count;
        @BindView(R.id.desc)TextView desc;
        public CommentsIconHolder(View itemView) {
            super(itemView);
            ButterKnife.bind(this,itemView);
        }

        @Override
        public void onBind(int position) {
            comment_count.setText(""+postMeta.getComment_count());
            views_count.setText(""+postMeta.getView_count());
            like_count.setText(""+postMeta.getLike_count());
            if(postMeta.getDesc()!=null){
                desc.setVisibility(View.VISIBLE);
                desc.setText(Html.fromHtml(postMeta.getDesc()));
            }
        }
    }

{% endhighlight %} 

In that case our onBindViewHolder routine of the adapter changes to the following.


{% highlight java %}

	@Override
    public void onBindViewHolder(CommentsAdapter.BaseHolder viewholder, int position) {
        viewholder.onBind(position);
    }

{% endhighlight %} 

`polymorphism` takes care of the rest!!



