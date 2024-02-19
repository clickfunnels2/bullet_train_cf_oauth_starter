# Bullet Train Click Funnels Starter

## Getting Started

1. You must have the following dependencies installed:

     - Ruby 3
          - See [`.ruby-version`](.ruby-version) for the specific version.
     - Node 19
          - See [`.nvmrc`](.nvmrc) for the specific version.
     - PostgreSQL 14
     - Redis 6.2
     - [Chrome](https://www.google.com/search?q=chrome) (for headless browser tests)

    If you don't have these installed, you can use [rails.new](https://rails.new) to help with the process.

2. Run the `bin/setup` script.
3. Start the application with `bin/dev`.
4. Visit http://localhost:3000.

## Information about Bullet Train
If this is your first time working on a Bullet Train application, be sure to review the [Bullet Train Basic Techniques](https://bullettrain.co/docs/getting-started) and the [Bullet Train Developer Documentation](https://bullettrain.co/docs).

### Render

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/bullet-train-co/bullet_train)

Clicking this button will take you to the first step of a process that, when completed, will provision production-grade infrastructure for your Bullet Train application which will cost about **$30/month**.

When you're done deploying to Render, you need to go into "Dashboard" > "web", copy the server URL, and then go into "Env Groups" > "settings" and paste the URL into the value for `BASE_URL`.

### Adding ClickFunnels OmniAuth

<!-- `chmod a+x ./bin/configure-click-funnels` -->

Run

`bin/configure-click-funnels` to generate the required models and migrations for
adding ClickFunnels as an OmniAuth option.

You can also view an example of the completed OmniAuth implementation on the
`completed-oauth-setup` branch.

> [!NOTE]
> There is currently a manual fix required to fix issues with the generated
> index names being too long.
>
> You will need to update these index names manually to succefully run the new
> migrations.
>
> Below are examples of how the migrations should look after updating the index
> names.

`_create_webhooks_incoming_oauth_click_funnels_account_webhooks.rb`
```ruby

class CreateWebhooksIncomingOauthClickFunnelsAccountWebhooks < ActiveRecord::Migration[7.1]
  def change
    create_table :webhooks_incoming_oauth_click_funnels_account_webhooks do |t|
      t.jsonb :data
      t.datetime :processed_at
      t.datetime :verified_at
      t.references :oauth_click_funnels_account, null: true, foreign_key: true, index: {name: "index_cf_webhooks_on_oauth_click_funnels_account_id"}

      t.timestamps
    end
  end
end

```

` _create_integrations_click_funnels_installations.rb`
```ruby
class CreateIntegrationsClickFunnelsInstallations < ActiveRecord::Migration[7.1]
  def change
    create_table :integrations_click_funnels_installations do |t|
      t.references :team, null: false, foreign_key: true
      t.references :oauth_click_funnels_account, null: false, foreign_key: true, index: {name: "index_cf_installations_on_oauth_click_funnels_account_id"}
      t.string :name

      t.timestamps
    end
  end
end

```
