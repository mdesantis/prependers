# Prependers

Prependers are a way to easily and cleanly extend third-party code via `Module#prepend`.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'prependers'
```

And then execute:

```console
$ bundle
```


Or install it yourself as:

```console
$ gem install prependers
```

## Usage

To define a prepender manually, simply include the `Prependers::Prepender.new` module. For instance,
if you have installed an `animals` gem and you want to extend the `Animals::Dog` class, you can
define a module like the following:

```ruby
module Animals::Dog::AddBarking
  include Prependers::Prepender.new

  def bark
    puts 'Woof!'
  end
end

Animals::Dog.new.bark # => 'Woof!'
```

### Autoloading prependers

If you don't want to include `Prependers::Prepender`, you can also autoload prependers from a path.
Here's the previous example, but with autoloading:

```ruby
# app/prependers/animals/dog/add_barking.rb
module Animals::Dog::AddBarking
  def bark
    puts 'Woof!'
  end
end

# somewhere in your initialization code
Prependers.load_paths(File.expand_path('app/prependers'))
```

You can pass multiple arguments to `#load_paths`, which is useful if you have subdirectories in
`app/prependers`:

```ruby
Prependers.load_paths(
  File.expand_path('app/prependers/controllers'),
  File.expand_path('app/prependers/models'),
  # ...
)
```

Note that, in order for autoprepending to work, the paths of your prependers must match the names
of the prependers you defined.

### Using a namespace

It can be useful to have a prefix namespace for your prependers. That way, you don't have to worry
about accidentally overriding any vendor modules. This is actually the recommended way to define
your prependers.

You can accomplish this by passing an argument when including the `Prependers::Prepender` module:

```ruby
module MyApp
  module Animals
    module Dog
      module AddBarking
        include Prependers::Prepender.new(MyApp)

        def bark
          puts 'Woof!'
        end
      end
    end
  end
end
```

If you use autoloading, you can pass the base namespace to `#load_paths`:

```ruby
Prependers.load_paths(File.expand_path('app/prependers'), namespace: MyApp)
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run
the tests. You can also run `bin/console` for an interactive prompt that will allow you to
experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new
version, update the version number in `version.rb`, and then run `bundle exec rake release`, which
will create a git tag for the version, push git commits and tags, and push the `.gem` file to
[rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/nebulab/prependers.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
