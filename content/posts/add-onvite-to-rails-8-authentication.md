---
title: "Add Onvite to Rails 8 Authentication"
date: 2025-01-08
tags: 
  - "go"
  - "ui"
  - "un"
---

This article was originally published on Rails Designer Build a SaaS

This is the third article on Building a SaaS with Ruby on Rails. This article continues where the previous one, adding sign up to Rails 8' authentication, stops.

Most SaaS products are used by multiple people from the same team or company. Although I would, when you start building your SaaS, urge you to keep it simple and just support one user on launch. I would álso urge you to build the blocks needed for teams in your app from the get-go. Having to deal with workspaces, teams and roles when you already have users is a bigger pain than you can imagine.

This article will focus on an important feature, and a metric you likely want to keep track off for new sign ups; invitations. Not just an invitation to use the app (though you can tweak it to do just that), but an invitation to the inviter's workspace.

As always the repo can be found here. Let's get right to it.

## Adding workspaces

The previous article on adding sign ups, left the option to add a Workspace on sign up. Let's add that now. It's simple really. All it is, for now, is a record with a `has_many :users`. All business records your users create will belong to the workspace, giving the users access to them (but that will be for another article).

Let's create the model first: `rails g model Workspace name:string`. Then update the existing User model by adding a **workpace\_id**: `rails g migration add_workspace_id_to_users workspace_id:integer`.

Update the new Workspace model like this:  

```
# app/models/workspace.rb
class Workspace < ApplicationRecord
  has_many :users, dependent: :destroy
end
```

Then the User model:  

```
class User < ApplicationRecord
  # …

  belongs_to :workspace

  # …
end
```

Let's also update **app/models/current.rb** for some nicety: add `delegate :workspace, to: :user`. This will allow you to do `Current.workspace` to get the user's workspace. Nice!

Head over the Signup class and the actual code to create a new workspace on sign up:  

```
class Signup
  include ActiveModel::Model
  include ActiveModel::Attributes

  def save
    # …
  end

  private

  def create_workspace_for(user)
    Workspace.create(name: "New Workspace").tap do |workspace|
      workspace.users << user
    end
  end
end
```

Alright. Now the pieces are already in place to invite others to the Workspace.

## Adding invites

What I am having in mind is the following:

1. Workspace "owner" (the User would created the Workspace) adds email from invitee;
2. Invitation model is created, with: email and inviter\_id;
3. After Invitation create, an email is sent to the email with an invite link;
4. On clicking the link, invitee sees a form with email and password;
5. Upon submit, an User model is created and attached to the Workspace.

Doesn't look all too bad when written out like this, right? Let's create the invitation model first: `rails g model Invitation workspace_id:integer inviter_id:integer email_address:string accepted_at:datetime`.

Let's update the newly created model:  

```
class Invitation < ApplicationRecord
  belongs_to :workspace
  belongs_to :inviter, class_name: "User"
end
```

Let's also update the Workspace model by adding `has_many :invitations, dependent: :destroy`. This will allow to list all (pending) invitations per Workspace.

In its most basic form there needs to be two controller with two actions each:

- one for creating the invitation by the workspace owner (actions: new and create);
- one for accepting the invitation by the invitee (actions: new and create).

Let's do the invitations creations first. It is simple really!  

```
class InvitationsController < ApplicationController
  def index
    @invitations = Current.workspace.invitations
  end

  def new
    @invitation = Invitation.new
  end

  def create
    if invitation = Invitation.create(invitation_params)
      InvitationsMailer.invite(invitation).deliver_later
    end

    redirect_to invitations_path
  end

  private

  def invitation_params
    params
      .expect(invitation: [ :email_address ])
      .with_defaults(
        inviter_id: Current.user.id, workspace_id: Current.user.workspace_id
      )
  end
end
```

Then add to the routes `resources :invitations, only: %w[index new create]`. Now it's also clear what views are needed:

**NB**: as with all articles in this series, the UI is left up to you. Head over the articles from Rails Designer, the components and check out the cool new Form Builder. It will give you beautiful form inputs by just typing `form.input :email_address`. 👀

Simple list of invitations:  

```
# app/views/invitations/index.html.erb
<ul>
  <% @invitations.each do |invitation| %>
    <li>
      <%= invitation.email_address %> - <%= invitation.accepted_at %>
    </li>
  <% end %>
</ul>
```

Then the form to create a new invitation:  

```
<%= form_for @invitation do |form| %>
  <%= form.text_field :email_address %>
  <%= form.submit %>
<% end %>
```

Simple enough! Let's create the mailer to send the invitation over:  

```
# app/mailers/invitations_mailer.rb
class InvitationsMailer < ApplicationMailer
  def invite(invitation)
    @invitation = invitation

    mail subject: "You are invited!", to: invitation.email_address
  end
end

# app/views/invitations_mailer/invite.html.erb
<p>
  Here is your invitation. Click this link to
  <%= link_to "get started", accept_invitation_url(token: @invitation.generate_token_for(:invitation)) %>.
</p>
```

This shows a couple things that needs to be added and create the functionality for `@invitation.generate_token_for(:invitation)` and the route, controller, class and views for `accept_invitation_url()`.

Let's do the easy one first, let's add the following to your **Invitation** Active Model:  

```
class Invitation < ApplicationRecord
  # …
  generates_token_for :invitation, expires_in: 7.days do
    accepted_at
  end
  # …
end
```

This is a feature introduced in Rails 7.1. This also gives the method find\_by\_token\_for() to look up the invitation as you will see in a moment. The nice thing, it will return `nil` if the **accepted\_at** column is set.

Let's speed through the boilerplate for accepting invitations and then look at the class to add the new user to the workspace.  

```
# app/controllers/accept_invitations_controller.rb
class AcceptInvitationsController < ApplicationController
  def new
    @invitation = Invitation.find_by_token_for(:invitation, params[:token])
  end

  def create
    AcceptInvitation.create(invitations_params)

    redirect_to root_path
  end

  private

  def invitation_params
    params.expect(accept_invitation: [ :email_address, :password ])
  end
end

# config/routes.rb
Rails.application.routes.draw do
  # …
  resources :accept_invitations, only: %w[new create]
end

# app/views/accept_invitations/new.html.erb
<%= form_with model: @invitation, url: accept_invitations_path do |form| %>
  <%= form.email_field :email_address %>
  <%= form.password_field :password %>
  <%= form.submit %>
<% end %>
```

Wow! That was fast. 🏎️ Let's create the **AcceptInvitation** class. It will be similar **form object** is done previously for **Signup**.  

```
class AcceptInvitation
  include ActiveModel::Model
  include ActiveModel::Attributes

  attribute :email_address, :string
  attribute :password, :string
  attribute :token, :string

  validates :email_address, presence: true
  validates :password, length: 8..128

  def save
    if valid?
      User.create!(email_address: email_address, password: password).tap do |user|
        update_invitation
        add_new user, to: invitation.workspace
      end
    end
  end

  def model_name
    ActiveModel::Name.new(self, nil, self.class.name)
  end

  private

  def update_invitation
    invitation.update(accepted_at: Time.current)
  end

  def add_new(user, to:)
    to.users << user
  end

  def invitation = Invitation.find_by_token_for(:invitation, token)
end
```

This class is the place to handle everything needed after the invite is accepted. I've left it to updating the **accepted\_at** column (remember how that expires the token) and adding the new user to the workspace. But you can add any action that makes sense for your business logic.

And there you have it. A simple way to add invites to your Rails 8 authentication generator.

Go to Source
