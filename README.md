# github-webhook-collector

Create a [Keen IO](https://keen.io/) webhook for all of your Github repositories.
Storing [Github events](https://developer.github.com/webhooks/#events) in Keen IO gives you the power to do analytics on Github activities over time.

### Setup

Clone and install dependencies:

``` shell
$ git clone git@github.com:dzello/github-webhook-collector.git
$ cd github-webhook-collector
$ gem install bundler
$ bundle install
```

Create a `.env` file with your Github and Keen IO information:

```
GITHUB_USERNAME=XXXXXXX
GITHUB_PASSWORD=XXXXXXX
KEEN_PROJECT_ID=XXXXXXX
KEEN_WRITE_KEY=XXXXXXXX
```

Substitute the X's for your actual credentials. (This file is in .gitignore, so it won't get checked in.)

### Usage

List existing webhooks pointing to `api.keen.io`:

``` shell
$ foreman run rake hooks:list
```

Create webhooks for all repositories you have admin access to:

``` shell
$ foreman run bundle exec rake hooks:create
```

Remove existing webhooks to `api.keen.io`:

``` shell
$ foreman run bundle exec rake hooks:remove
```

### Contributing

Contributions are welcome!
