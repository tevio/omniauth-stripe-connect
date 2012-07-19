# Omniauth::StripePlatform

Stripe Platform OAuth2 Strategy for OmniAuth 1.0.

Supports the OAuth 2.0 server-side and client-side flows.
Read the Stripe Platform docs for more details: https://stripe.com/platform

## Installation

Add this line to your application's Gemfile:

    gem 'omniauth-stripe-platform'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install omniauth-stripe-platform

## Usage

OmniAuth::Strategies::StripePlatform is simply a Rack middleware. Read the OmniAuth
1.0 docs for detailed instructions: https://github.com/intridea/omniauth.

Here's a quick example, adding the middleware to a Rails app in
`config/initializers/omniauth.rb`:

```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :stripe_platform, ENV['STRIPE_PLATFORM_CLIENT_ID'], ENV['STRIPE_SECRET']
end
```

Your `STRIPE_PLATFORM_CLIENT_ID` is application-specific and your `STRIPE_SECRET` is account-specific and may also be known as your Stripe API key or Stripe Private key.

Then you can hit `/auth/stripe_platform`

### Ruby on Rails apps with Devise

After setting up Devise to use OmniAuth, you only need to add the following line of code to handle the OAuth2 part of Stripe Platform.

```ruby
# Put this in config/initializers/devise.rb with the rest of your Devise configuration
config.omniauth :stripe_platform, ENV['STRIPE_PLATFORM_CLIENT_ID'], ENV['STRIPE_SECRET_KEY'], {:scope => 'read_write'} # or :scope => 'read_only'
```

Now your done!

## Auth Hash

Here is an example of the Auth Hash you get back from calling `request.env['omniauth.auth']`:

```ruby
{
  "provider"=>"stripe_platform",
  "uid"=>"<STRIPE_USER_ID>",
  "info"=>
  {
    "scope"=>"read_only", # or "read_write"
    "livemode"=>false,
    "access_token"=>nil,
    "expires_in"=>nil,
    "stripe_publishable_key"=>"<STRIPE_PUBLISHABLE_KEY>"
  },
  "credentials"=>
  {
    "token"=>"<TOKEN>",
    "refresh_token"=>"REFRESH_TOKEN",
    "expires_at"=>1341608127,
    "expires"=>true
  },
  "extra"=>
  {
    "raw_info"=>
    {
      "scope"=>"read_only",
      "livemode"=>false,
      "token_type"=>"bearer",
      "stripe_user_id"=>"<STRIPE_USER_ID>",
      "stripe_publishable_key"=>"<STRIPE_PUBLISHABLE_KEY>"
    }
  }
}
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
