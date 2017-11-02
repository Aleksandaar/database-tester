[![Gem Version](https://badge.fury.io/rb/database_tester.svg)](http://badge.fury.io/rb/database_tester)

# Database::Tester

Testing database methods made easy.

The gem provides `RSpec` tests for most of the ActiveRecord methods. Tested on `MySQL`, `PostgreSQL`, `Derby` and `SpliceMachine` databases.

## Installation

Add this line to your application's Gemfile, under `test` group:

```ruby
group :test do
  gem 'database_tester'
end
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install database_tester

In your `spec_helper.rb` add on top of your file:

    require 'database/tester'

## Usage

The gem is mostly a sum of RSpes shared examples. In order to use the gem's functions, write `RSpec` using:

```ruby
let(:model) { described_class }
subject(:item) { create model } # (You can use "let" as well)
```

- `:model` is used to test the database functions agains the model that is specified in this variable.

- `:item` is the actual database record against which the tests will be run. This object is mostly automatically created within most of the tests, but it is needed for testing CRUD operations, so make sure it it initialized when testing `create`, `update` etc. methods.


## Examples and List of Methods:

```ruby
describe Company do
  let(:model) { described_class }

  it { should have_db_column(:name).of_type(:string) }
  it { should have_many(:users) }
  it { should have_many(:profiles).through(:users) }
  it { should have_one(:address) }


  describe 'CRUD operations' do
    subject(:item) { create model }

    it_behaves_like 'model create'
    it_behaves_like 'model update', :name, 'New Company Name'
    it_behaves_like 'model readonly update', :description, 'Company Description'
    it_behaves_like 'model destroy'
    it_behaves_like 'model create_with', :name, 'New Company', :description, 'This Is A New Company'
  end

  describe 'Validation' do
    it_behaves_like 'model validation', :name
  end

  describe 'Associations' do
    it_behaves_like 'has_many association', User
    it_behaves_like 'has_one association', Profile
    it_behaves_like 'belongs_to association', Organization, true
    it_behaves_like 'polymorphic association', :address, Address
    it_behaves_like 'has_many through association', Profile, User
    it_behaves_like 'habtm association', Tag
  end

  describe 'Selectors' do
    it_behaves_like 'selector', :all_records
    it_behaves_like 'selector', :ordered
    it_behaves_like 'selector', :reverse_ordered
    it_behaves_like 'selector', :limited
    it_behaves_like 'selector', :selected
    it_behaves_like 'selector', :grouped, group_by: :name, options: { name: 'New Name' }
    it_behaves_like 'selector', :offsetted
    it_behaves_like 'selector all with update', :name, { name: 'New Company' }
    it_behaves_like 'find selector'
    it_behaves_like 'join and include query', User
    it_behaves_like 'distinct selector', :name
    it_behaves_like 'eager_load selector', :users
    it_behaves_like 'preload selector', :users
    it_behaves_like 'references selector', :users
    it_behaves_like 'reverse_order selector'
    it_behaves_like 'find_by selector', :name, 'name1', 'name2'
    it_behaves_like 'range conditions'
    it_behaves_like 'subset conditions'
    it_behaves_like 'find_or_create_by selector', :name, 'Company1'
    it_behaves_like 'find_or_create_by! selector', :name, 'Company1'
    it_behaves_like 'find_or_initialize_by selector', :name, 'Company1'
    it_behaves_like 'select_all selector'
    it_behaves_like 'pluck selector', :name
    it_behaves_like 'ids selector'
    it_behaves_like 'exists? selector', :name, 'val1', 'val2'
    it_behaves_like 'minimum selector', :name, ['3', '5', '1', '4', '2']
    it_behaves_like 'raw sql selector', :name, 'TestName'
  end

  describe 'Additional functions' do
    it_behaves_like 'pessimistic locking'
    it_behaves_like 'extending scope'
    it_behaves_like 'none relation'
  end
end
```

## Testing

To test the code run in your project directory:

```ruby
bundle exec rspec spec/
```

## Contributing

Bug reports and pull requests are always welcome.

1. Fork it ( https://github.com/aleksandaar/database_tester/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request


## Ruby versions

   Currently tested on **Ruby** `2.3.1` and **JRuby** `9.1.5.0`


## Credits

`Database Tester` is developed and maintained by [Aleksandar Zoric](https://github.com/aleksandaar "Aleksandar Zoric")

## License

MIT License. See LICENSE for details.