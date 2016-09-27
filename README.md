# RailsSelectOnIncludes

This gem solves issue in rails: https://github.com/rails/rails/issues/15185 for base_class.

It was impossible to select virtual attributes to object from its relations or any other way 
when using includes and where. 

Example from upper rails issue: 

```ruby
post = Post.includes(:comments).select("posts.*, 1 as testval").where( SOME_CONDITION ).first
post.testval # Undefined method!
```

This gem solves problem for base class i.e. 

```ruby
post = Post.includes(:comments).select("posts.*, 1 as testval").where( SOME_CONDITION ).first
post.testval # 1
```

but of course it doesn't include virtual attributes in included relations

```ruby
post = Post.includes(:comments).select("posts.*, 1 as testval").where( SOME_CONDITION ).first
post.comments.first.testval # Undefined method!
```

# RailsSelectOnIncludes (Рус)

Данный gem решает проблему в рельсах с виртуальными аттрибутами при использовании includes, 
когда рельсы собирают в запрос в joins с алиасами на все аттрибуты. В настоящий момент в модель не собираются 
никаким боком виртуальные аттрибуты.

В частности проблема описана здесь: https://github.com/rails/rails/issues/15185 

В коде примера по ссылке выше это выглядит так: 

```ruby
post = Post.includes(:comments).select("posts.*, 1 as testval").where( SOME_CONDITION ).first
post.testval # Undefined method!
```

Данный gem решает эту проблему для базового класса, т.е.:  

```ruby
post = Post.includes(:comments).select("posts.*, 1 as testval").where( SOME_CONDITION ).first
post.testval # 1
```

Но это не касается, объектов попадающих под инклюд:

```ruby
post = Post.includes(:comments).select("posts.*, 1 as testval").where( SOME_CONDITION ).first
post.comments.first.testval # Undefined method!
```


## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rails_select_on_includes'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rails_select_on_includes

## Usage

Works out of the box, gently monkey-patching base-class alias columns. It not affecting query creation, 
since query already contains all columns, i.e. to_sql returns same string.
Works with selection in all formats:

1 'table_name.column' or 'table_name.column as column_1' with distinct on also

2 { table_name: column } or { table_name: [column1, column2] }

3 { table_name: 2 } where 2 relates to upper syntax

## Usage (рус)

Работает из коробки, нежно манки-патча алиасы прямо перед инстанцированием коллекции, не влияет на создаваемый запрос в БД т.е to_sql не меняется.  

Поддерживает select в следующих форматах :

1 'table_name.column' or 'table_name.column as column_1' with distinct on also

2 { table_name: column } or { table_name: [column1, column2] }

3 { table_name: 2 } where 2 relates to upper syntax

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/rails_select_on_includes.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

