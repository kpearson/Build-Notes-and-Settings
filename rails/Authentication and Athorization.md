#Authorization and Authentication in Rails

##Authentication
Todo:

##Authorization
###Sessions
Sessions are created by the sessions_controller.rb. They do not have a model. They are stored in the browser.
session#new is the login view.
session#create is the action checking teh cerdentials and finding the user in the datatbase. The final part is to redirect to the users#show.
(users_path(user))
session#destroy 

###before_action
Before_action can contain many things.  In the context of *authorization* placeing "before_action :authorize" in the ApplicationController makes it available throu out the app.

###ApplicationController
ApplicationController contains the current_user and authorize methods makign them available every where in the app

####current_user and authorize methods
Usualy are often found togeter in the *application_controller*

#####current_user method
```@current_user ||= User.find(session[:user_id]) if session[:user_id]```
Ass

#####authorize method
```redirect_to login_path, alert: "Not authorized" if current_user.nil?```

###Cancancan gem
Manages verious abilities throught the application.
include ```gem 'cancancan'``` in the gem file.
then run ```$ rails g cancan:ability```
Provides a file called *ability.rb* insided app/models/

cancan DSL
*can* Methods -  :read, :manage, :update, :create, :destroy are common ones but can be anything.

    class Ability
      def initialize(user)
        user ||= User.new # guest user (not logged in)
        can :read, Photo, Photo.unowned do |photo|
          photo.groups.empty?
        end
      end
    end

authorize!
@user = User.find(params[:id])
authorize! :read, @user

###enum role attribute
in the model in which the object has a role. (User as in defaule, admin, moderator. or )
```enum role: %w(default admin)```




