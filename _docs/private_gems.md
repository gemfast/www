---
title: Private Gems
subtitle: RubyGems can be configured to publish gems to Gemfast and to use packages stored on Gemfast as dependencies in a Ruby project.
tags: [features, featured]
author: greg
order: 3
---

### Creating a Private Gems Token

In order to install and manage private gems on a Gemfast server, a private gems token is required. Private gem tokens can be generated after successfully logging in to a Gemfast server.

Private gem tokens are different from the JWT token that authenticates users to the `/admin/api/v1` endpoints.

<aside>
  <p>
    These examples assume the server is configured to use the <a href="/docs/local_auth">local authentication</a> strategy.
  </p>
</aside>

To login, run the following cURL command:

```bash
curl -X POST \
-H 'Content-Type: application/json' \
-d '{"username": "USERNAME", "password": "PASSWORD"}' \
https://GEMFAST_HOST/admin/api/v1/login
```

You must replace:

* `GEMFAST_HOST` with the hostname of your self-hosted Gemfast server.
* `USERNAME` with your Gemfast username.
* `PASSWORD` with your Gemfast password.

Save the `token` for the next request:

```json
{
  "code": 200,
  "expire": "2023-07-12T03:18:11-04:00",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE2ODkxNDYyOTEsImlkIjoiYWRtaW4iLCJvcmlnX2lhdCI6MTY4OTEwMzA5MSwicm9sZSI6ImFkbWluIn0.PZ_B8pqxqN9-hQfKQ5F0rHXmlNybQYByaeLSMHPaMog"
}
```

Next, generate a token for that will be used by `bundler` or the `gem` command to authenticate with gemfast:

```bash
curl -X POST \
-H 'Authorization: Bearer JWT_TOKEN' \
https://GEMFAST_HOST/admin/api/v1/token
```

You must replace:

* `JWT_TOKEN` with the `token` value from above.

The `token` field in the response is the private gem token which can be used with RubyGems and bundler.

```
{
  "username": "bobvance",
  "token": "o8xh5cjai32tyn6gv9wzsdmru0fk71e4"
}
```

### Authenticating with a Private Gems Token

Gemfast supports tokens with granular permissions. A user's role determines what they can do with a private gems token. By default, `read` users can only install gems and `write` users can both install and publish new gems. Only an `admin` user can delete (yank) private gems.

#### Publishing Private Gems to Gemfast

In order to push gems to Gemfast, add an entry to `~/.gem/credentials`. 

```yaml
:gemfast: USERNAME:TOKEN
```

You must replace:

* `USERNAME` with your Gemfast username.
* `TOKEN` with your Gemfast private gems token.

#### Installing Private Gems from Gemfast

To install gems, you need to authenticate using your private gems token by updating your gem sources to include https://USERNAME:TOKEN@GEMFAST_HOST/NAMESPACE. 

To use private gems with `bundler`, add a source scope to your `Gemfile`:

```ruby
source "https://rubygems.org"

gem "rails"

source "https://USERNAME:TOKEN@GEMFAST_HOST/NAMESPACE" do
  gem "GEM_NAME"
end
```

You must replace:

* `GEMFAST_HOST` with the hostname of your self-hosted Gemfast server.
* `USERNAME` with your Gemfast username.
* `TOKEN` with your Gemfast private gems token.
* `NAMESPACE` with the value of `private_gems_namespace` from `/etc/gemfast/gemfast.hcl`. By default the namespace is `private`

It is also possible to set the `USERNAME` and `TOKEN` values in the `bundler` configuration so they are not present in the `Gemfile`:

```bash
bundle config https://GEMFAST_HOST/NAMESPACE USERNAME:TOKEN
```

Another way is to provide multiple sources in your `Gemfile` which will be checked in order of occurence. In the following example, Gemfast private gems will be used over gems from RubyGems if they exist on the Gemfast server.

```ruby
source "https://USERNAME:TOKEN@GEMFAST_HOST/NAMESPACE"
source "https://rubygems.org"

gem "rails"
gem "GEM_NAME"
```

If you need to install private gems without `bundler`, Gemfast also works with the `gem` CLI:

```bash
gem install GEM_NAME --source https://USERNAME:TOKEN@GEMFAST_HOST/NAMESPACE
```

You can also set the source globally so the `--source` flag does not need to be specified when installing gems:

```bash
gem sources -a https://USERNAME:TOKEN@GEMFAST_HOST/NAMESPACE
```

#### Publishing a Private Gem to Gemfast

Gemfast supports the existing RubyGems API for publishing gems.

To publish a gem, first build it using the `gem` CLI:

```bash
gem build GEM_NAME.gemspec
```

Next, push the gem:

```bash
gem push --key gemfast \
--host https://GEMFAST_HOST/NAMESPACE \
pkg/GEM_NAME-0.0.1.gem
```

You must replace:

* `GEMFAST_HOST` with the hostname of your self-hosted Gemfast server.
* `NAMESPACE` with the value of `private_gems_namespace` from `/etc/gemfast/gemfast.hcl`. By default the namespace is `private`

Gemfast also supports "Gem in a Box" style uploads:

```bash
gem install geminabox

gem inabox -g https://USERNAME:TOKEN@GEMFAST_HOST/NAMESPACE \
pkg/GEM_NAME-0.0.1.gem
```

You must replace:

* `GEMFAST_HOST` with the hostname of your self-hosted Gemfast server.
* `USERNAME` with your Gemfast username.
* `TOKEN` with your Gemfast private gems token.
* `NAMESPACE` with the value of `private_gems_namespace` from `/etc/gemfast/gemfast.hcl`. By default the namespace is `private`

#### Deleting a Private Gem from Gemfast

Gemfast supports the existing RubyGems API for yanking gems.

To delete (yank) a private gem, use the `gem` CLI:

```bash
gem yank --key gemfast \
--host https://GEMFAST_HOST/NAMESPACE \
GEM_NAME -v VERSION
```

You must replace:

* `GEMFAST_HOST` with the hostname of your self-hosted Gemfast server.
* `NAMESPACE` with the value of `private_gems_namespace` from `/etc/gemfast/gemfast.hcl`. By default the namespace is `private`

It is also possible to delete a private gem using the API directly.

```bash
curl -X DELETE \
-H 'Authorization: JWT_TOKEN' \
https://GEMFAST_HOST/NAMESPACE/api/v1/gems/yank?gem=GEM_NAME&version=VERSION
```

You must replace:

* `JWT_TOKEN` with the `token` value received from a successful login to `/admin/api/v1/login`.
* `GEMFAST_HOST` with the hostname of your self-hosted Gemfast server.
* `USERNAME` with your Gemfast username.
* `TOKEN` with your Gemfast private gems token.
* `NAMESPACE` with the value of `private_gems_namespace` from `/etc/gemfast/gemfast.hcl`. By default the namespace is `private`