*Test Driven Development With Chef*

##FoodCritic
(FoodCritic)[http://acrmp.github.io/foodcritic/]

* Exclude certain rules e.g. FC002: foodcritic --tags ~FC002
* Exclude certain rules in code via comment: # ~FC002

##Best Practice
Resource based testing - good example of how to do it right is 
[Sean O'Mara's httpd cookbook refactor](https://github.com/chef-cookbooks/httpd)
[Sean O'Mara's mysql cookbook refactor](https://github.com/chef-cookbooks/mysql)

##Rubocop
[Rubocop - Static Ruby code analyzer](https://github.com/bbatsov/rubocop)

##ChefSpec

[ChefSpec Sane Documentation With Examples](http://www.rubydoc.info/github/acrmp/chefspec/frames)

* Add coverage report: ```at_exit { ChefSpec::Coverage.report! }```
