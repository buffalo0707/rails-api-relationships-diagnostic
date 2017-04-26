# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    A join table is needed when doing a many to many relatonship without repeating
    data in a given resource. It also lets you add additional data to give the
    join additional meaning (such as date when looking at the loans example from
    the Library api)
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    Profiles: id (primary key), given_name (text), surname(text), email (text)
    Favorites: id (primary key), profile_id (integer, foreign key),
      movie_id (integer, foreign key)
    Movies: id (primary key), title (text), release_date(date), length (integer)


    A Profile can have many movies through Favorites. Movies belong to many
    Profiles through Favorites. A Favorite belongs to one Profile, and a Profile
    can have many Favorites. A Favorite has one Movie, and a Movie belongs
    to many Favorites
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through: :favories
    has_many :favories
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :movie, inverse_of: :favorites
  belongs_to :profile, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    The Serializer defines what data will be available in the JSON that is returned
    to the client.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes: :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bin/rails generate scaffold favorite profile:references movie:references
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    Dependent: Destroy helps inforce referential integretity by removing a
    row from a join table when any of the dependent resources are deleted. In
    this example, it should be added to the Movie and Profile models like this:
      has_many :favorites, dependent: :destroy
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
  A book share program where users can own loan books out and/or borrow from others
    User
    Books
    Loans
    User can have many Books, Books belong to one user
    Users have many Books through Loans
    Books have many users through Loans
    Users can have many Loans, Loans belong to one User
    Books can belong to many Loans, Loans can have one Book.

  ```
