# Corner Stones

[![Build Status](https://secure.travis-ci.org/senny/corner_stones.png)](http://travis-ci.org/senny/corner_stones)
[![Code Climate](https://codeclimate.com/badge.png)](https://codeclimate.com/github/senny/corner_stones)

assists you in building PageObjects to make your acceptance tests more object oriented.

## Installation

``` terminal
$ gem install corner_stones
```

or in your **Gemfile**

``` ruby
gem 'corner_stones'
```

## Examples

a lot of examples can be found in the [integration specs](https://github.com/senny/corner_stones/tree/master/spec/integration).
Some features of corner_stones are listed below.

### Tabs

```ruby
tabs = CornerStones::Tabs.new('.tab-navigation')
tabs.open('Details') # open a tab
```

```ruby
tabs = CornerStones::Tabs.new('.tab-navigation')
tabs.extend(CornerStones::Tabs::ActiveTracking)

tabs.open('About') # open a tab and verify that the opened tab is active
tabs.assert_current_tab_is('Main') # verify that the tab 'Main' is active
```

### Flash Messages

```ruby
flash = CornerStones::FlashMessages.new
flash.assert_flash_is_present(:notice, 'Article saved') # verify that a given flash message is present
```

### Tables

```ruby
table = CornerStones::Table.new('.articles')
table.rows # returns an array of rows. Each row is represented as a row object.
table.row('Title' => 'Management') # returns the row object for the row with 'Management' in the 'Title' column
```

A row object has two primary methods: `#node` is a reference to the capybara node of the row and `#attributes` is a hash
with the following structure: ({Table Header} => {Cell Value})

The following extensions are available for the `Table`:

```ruby
table = CornerStones::Table.new('.articles')
table.extend(CornerStones::Table::SelectableRows)
table.extend(CornerStones::Table::DeletableRows)

table.row('Created at' => '01.12.2001').select # select the row, which has '01.12.2001' in the 'Created at' column
table.row('ID' => '9').delete # delete the row, which contains '9' in the 'ID' column
table.values("ID") # => ["1", "7", "9"]
```

### Forms

```ruby
form = CornerStones::Form.new('.new-article')
form.fill_in_with('Title' => 'Some Article', 'Author' => 'C. J.') # fill out the form
form.submit # submit the form using the 'Save' button
form.submit(:button => 'Save Article') # submit the form using the 'Save Article' button

form.process(:fill_in => {'Title' => 'Some Article', 'Author' => 'C. J.'},
             :button => 'Save Article') # fill out + submit
```

```ruby
form = CornerStones::Form.new('.update-article')
form.extend(CornerStones::Form::WithInlineErrors)

form.errors # returns an Array of form errors
form.assert_has_no_errors # verify that the form was submitted correctly
form.submit # verifies that the form has no errors
form.submit(:assert_valid => false) # do not veirfy that no errors were present
```

## Contributors

* Michael Stämpfli (https://github.com/stmichael)
* Björn Bur (https://github.com/bjoernbur)

If you want to help out, please start by reading the [contribution guidelines](CONTRIBUTING.md).

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/101b5a36327228500581193f9d91dfe3 "githalytics.com")](http://githalytics.com/senny/corner_stones)
