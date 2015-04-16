# mocks

General purpose mocking library for Crystal.

## Installation

Add it to `Projectfile`

```crystal
deps do
  github "waterlink/mocks.cr"
end
```

## Usage

```crystal
require "mocks"
```

### Partial double

```crystal
class Example
  def say_hello(name)
    "hey, #{name}"
  end
end

create_mock Example do
  mock say_hello(name)
  # or
  # mock instance.say_hello(name)
end

example = Example.new
allow(example).to receive(say_hello("world")).and_return("hello, world!")

example.say_hello("world")    #=> "hello, world!"
example.say_hello("john")     #=> "hey, john"
```

### Double

```crystal
create_double "OtherExample" do
  mock say_hello(name)

  # here `instance.` is required because of assign operator and syntax
  # parsing
  mock instance.greeting=(value)
end

example = double("OtherExample", returns(say_hello("world"), "hello world!"))
allow(example).to receive(instance.greeting=("hey")).and_return("hey")

example.say_hello("world")     #=> "hello world!"
example.say_hello("john")      #=> Mocks::UnexpectedMethodCall: #<Mocks::Doubles::OtherExample:0x109498F00> received unexpected method call say_hello["john"]
```

### Instance double

**Not implemented**

After defining `Example`'s mock with `create_mock` you can use it as an instance_double:

```crystal
example = instance_double(Example, returns(say_hello("world"), "hello, world!"))

example.say_hello("world")     #=> "hello world!"
example.say_hello("john")      #=> Mocks::UnexpectedMethodCall: #<Example:0x109498F00> received unexpected method call say_hello["john"]
```

### Class double

TODO: Come up with usage for class doubles (if they are needed at all)

## Development

TODO: Write instructions for development

## Contributing

1. Fork it ( https://github.com/waterlink/mocks.cr/fork )
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request

## Contributors

- [waterlink](https://github.com/waterlink) Oleksii Fedorov - creator, maintainer