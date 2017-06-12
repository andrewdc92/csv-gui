# FriendlyCSV
sanitize cold email merge variables before you hit send.

don't code? https://friendly-csv.herokuapp.com

<p align="center">
  <img src=https://raw.githubusercontent.com/ryanckulp/friendly_csv/master/app/assets/images/sanitize-in-action.gif alt="Sanitize a CSV"/>
</p>

### why?

* merging {{ first_name }} but your data says "John Smith"
* merging {{ company_name }} but your data says "Solstice Equity Partners, *Inc.*"
* merging {{ first_name }} but your data says "JIMMY"
* etc.

### getting started

1. `$ rake db:setup && rake db:migrate`
2. `$ rails s`

### todo

1. add example.csv to demonstrate how it works
2. styles (fonts, buttons, etc)
3. let user send leads to their email
4. what else?
