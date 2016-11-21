How to do Ajax in rails ??

Post your Js file will look like that :
```js
$.ajax({
    method: 'POST',
    url: "/projects/vote_project/" + $project,
    data: {
      social_media: 'facebook',
      message: $message,
    },
    success: function(data) {
      $('#myModal_fb').css('display', "none");
      $('#btn_fb').css('pointer-events', 'none');
      $('#btn_fb').css('background', '#7aa3f7');
    },
    error: function(data) {
    }
  });
```

And the controller will look like :
```ruby
def vote
    if @user.like!(@project, { social_media: params[:social_media], message: params[:message] })
      respond_to do |format|
        format.json { render json: "true".to_json }
      end
    else
      flash[:error] = "Erreur lors du vote"
      respond_to do |format|
        format.json { render json: "false".to_json }
      end
    end
```
