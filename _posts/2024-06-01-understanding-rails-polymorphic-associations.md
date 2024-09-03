---
date: 2024-06-01 12:00:00
layout: post
title: Understanding Rails Polymorphic Associations
subtitle:
description:
image: "/assets/img/rails_4.jpg"
optimized_image:
category: Rails
tags:
  - Rails
  - Rails Spotlight
author: debashisbiswal
---

**Polymorphic associations** allow a model to belong to more than one other model using a single association. This is particularly useful when you want a single model to be associated with multiple other models without having to create multiple associations or tables.

#### Use Case Scenario

Imagine you have three models: `Post`, `Image`, and `Comment`. You want to allow users to comment on both `Post` and `Image`. Instead of creating separate associations for `Post` and `Image` in the `Comment` model, you can use a polymorphic association.

### Models Setup

1. **Post Model:**
   ```ruby
   class Post < ApplicationRecord
     has_many :comments, as: :commentable
   end
   ```

2. **Image Model:**
   ```ruby
   class Image < ApplicationRecord
     has_many :comments, as: :commentable
   end
   ```

3. **Comment Model:**
   ```ruby
   class Comment < ApplicationRecord
     belongs_to :commentable, polymorphic: true
   end
   ```

### Database Schema

The `comments` table will have two additional columns: `commentable_type` and `commentable_id`. These columns allow the `Comment` model to be associated with both the `Post` and `Image` models.

```ruby
create_table :comments do |t|
  t.text :body
  t.references :commentable, polymorphic: true, index: true
  t.timestamps
end
```

- `commentable_id` stores the ID of the associated record (either `Post` or `Image`).
- `commentable_type` stores the class name of the associated model (either `Post` or `Image`).

### Usage In Action

Creating a comment for a `Post`:
```ruby
post = Post.find(1)
comment = post.comments.create(body: "Great post!")
```

Creating a comment for an `Image`:
```ruby
image = Image.find(1)
comment = image.comments.create(body: "Nice image!")
```

### Diagram

Here’s a simple diagram to illustrate how polymorphic associations work:

```plaintext
+------------------+           +----------------+
|      Post        |           |      Image      |
|------------------|           |----------------|
| id: integer      |           | id: integer     |
| title: string    |           | url: string     |
| ...              |           | ...             |
+------------------+           +----------------+
         |                          |
         |                          |
         |                          |
         |                          |
         |                          |
         |            +-------------+-----------+
         |            | Comment                   |
         |            |--------------------------|
         +----------->| id: integer               |
                      | body: text                |
                      | commentable_id: integer   |
                      | commentable_type: string  |
                      +--------------------------+
```

### Explanation

- The `Comment` model can belong to either a `Post` or an `Image`.
- The `commentable_type` field determines the type of model (either `Post` or `Image`).
- The `commentable_id` field stores the ID of the associated record in that model.

### Benefits of Polymorphic Associations

1. **Flexibility:** You can associate multiple models with a single model.
2. **Reduced Redundancy:** Instead of creating separate associations or tables, you manage relationships using a single interface.
3. **Simplified Schema:** Only one polymorphic association is needed instead of multiple foreign keys.

### Conclusion

Polymorphic associations are a powerful feature in Rails that allow for more flexible and DRY (Don't Repeat Yourself) code. They are particularly useful in scenarios where a model needs to interact with multiple other models in a similar way, like comments, tags, or likes.
