# DimensionValidation

DimensionValidation gem adds dimension validation to ActiveModel.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'dimension_validation'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install dimension_validation

## Usage

This validation is only applicable to attributes with width and height.

If you are using `CarrierWave`, can define a base uploader, as follows.
```
class BaseUploader < CarrierWave::Uploader::Base
  attr_reader :width, :height
  before :cache, :capture_size

  def capture_size(file)
    if version_name.blank?
      if file.path.nil?
        img = ::MiniMagick::Image::read(file.file)
        @width = img[:width]
        @height = img[:height]
      else
        @width, @height = `identify -format "%wx %h" #{file.path}`.split(/x/).map(&:to_i)
      end
    end
  end
end
```

### Available options
```
dimensions: width, height, aspect_ratio
options: greater_than, greater_than_or_equal_to, equal_to, less_than, less_than_or_equal_to
```

### Example
```
validates :avatar, dimension: { width: { equal_to: 240 }, height: { equal_to: 240 } }
validates_dimension_of :avatar, width: { equal_to: 240 }, height: { equal_to: 240 }
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/aspirewit/dimension_validation. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
