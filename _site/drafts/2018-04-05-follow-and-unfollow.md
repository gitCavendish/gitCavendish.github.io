user.rb

```ruby
class User < ApplicationRecord
  has_many :forward_relationships, class_name: "Relationship", foreign_key: :follower_id
  has_many :backward_relationships, class_name: "Relationship", foreign_key: :followed_id

  has_many :followings, through: :forward_relationships
  has_many :followers, through: :backward_relationships

  def follow(other_user)
    self.forward_relationships.create(followed_id: other_user.id)
  end

  def unfollow(other_user)
    self.forward_relationships.find_by(followed_id: other_user.id).destroy
  end
end
```

relationship.rb

```ruby
class Relationship < ApplicationRecord
  belongs_to :follower, class_name: "User" #, foreign_key: :followed_id
  belongs_to :following, class_name: "User", foreign_key: :followed_id
end
```
---

错误思路演示

user.rb

```ruby
class User < ApplicationRecord
  has_many :relationships # which key in Relationship as foreign_key???
  has_many :followings, through: :relationships, foreign_key: :follower_id, class_name: "User"
  has_many :followers, through: :relationships, foreign_key: :followed_id, class_name: "User"
end
```

relationship.rb

```ruby
class Relationship < ApplicationRecord
  belongs_to :follower, class_name: "User" #, foreign_key: :followed_id
  belongs_to :following, class_name: "User", foreign_key: :followed_id
end

```
