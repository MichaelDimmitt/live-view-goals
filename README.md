## live-view-goals

## collab site... two clients going at once we see the communication. 
1) https://dennisbeatty.com/2019/04/24/how-to-create-a-todo-list-with-phoenix-liveview.html
2) https://github.com/dnsbty/live_view_todos

## validation ... 
1) https://elixircasts.io/phoenix-live-view
2) 

## mccord talk 
explaining why live view can be better than elixir channels and umbrella-project example:
<br/>talk: https://www.youtube.com/watch?time_continue=6&v=8xJzHq8ru0M
<br/>example: https://github.com/chrismccord/phoenix_live_view_example

## setup asdf the right way:
<br/>https://www.cogini.com/blog/using-asdf-with-elixir-and-phoenix/
<br/>below is the mac instructions, see the link for the linux instructions.

```bash
{
  brew install asdf;
  echo -e '\n. $(brew --prefix asdf)/asdf.sh' >> ~/.bash_profile;
  echo -e '\n. $(brew --prefix asdf)/etc/bash_completion.d/asdf.bash' >> ~/.bash_profile;
  { brew install coreutils automake autoconf openssl libyaml readline libxslt libtool; };
  { brew install unixodbc wxmac gpg; brew cask install java };
  { bash ~/.asdf/plugins/nodejs/bin/import-release-team-keyring };
  { asdf plugin-add erlang; asdf plugin-add elixir; asdf plugin-add nodejs; };
  { asdf install erlang 21.3; asdf install elixir 1.8.1; asdf install nodejs 10.15.3; };
  { mix local.hex --if-missing --force };
  { mix local.rebar --if-missing --force };
  { asdf global erlang 21.3; asdf global elixir 1.8.1; asdf global nodejs 10.15.3; };
  { mix deps.get; mix deps.compile; mix compile; mix ecto.setup; };
};
```
## compatability check for elixir and erlang:
https://hexdocs.pm/elixir/master/compatibility-and-deprecations.html#compatibility-between-elixir-and-erlang-otp

## live-view breakdown:
proj/lib/demo_web/index.html.eex: 
<br/>`<li><%= link "Thermostat", to: Routes.live_path(@conn, DemoWeb.ThermostatLive) %></li>`

proj/lib/demo_web/router.ex:
```elixir
defmodule DemoWeb.Router do
  use DemoWeb, :router
  import Phoenix.LiveView.Router
  pipeline :browser do
    ...
    plug Phoenix.LiveView.Flash
    ...
  end
  scope "/", DemoWeb do
    ...
    live "/thermostat", ThermostatLive
    ... 
  end
```

proj/lib/demo_web/live/thermostat_live.ex: 
```elixir
def render(assigns) do
    ~L"""
    <div class="thermostat">
      <div class="bar <%= @mode %>">
        <a href="#" phx-click="toggle-mode"><%= @mode %></a>
        <span><%= strftime!(@time, "%r") %></span>
      </div>
      <div class="controls">
        <span class="reading"><%= @val %></span>
        <button phx-click="dec" class="minus">-</button>
        <button phx-click="inc" class="plus">+</button>
        <span class="weather">
          <%= live_render(@socket, DemoWeb.WeatherLive) %>
        </span>
      </div>
    </div>
    """
  end
  def mount(_session, socket) do
    if connected?(socket), do: :timer.send_interval(100, self(), :tick)
    {:ok, assign(socket, val: 72, mode: :cooling, time: :calendar.local_time())}
  end
```

proj/lib/demo_web/live/weather_live.ex
```elixir
defmodule DemoWeb.WeatherLive do
  use Phoenix.LiveView

  def render(assigns) do
    ~L"""
    <div>
      <form phx-submit="set-location">
        <input name="location" placeholder="Location" value="<%= @location %>"/>
        <%= @weather %>
      </form>
    </div>
    """
  end

  def mount(_session, socket) do
    send(self(), {:put, "Austin"})
    {:ok, assign(socket, location: nil, weather: "...")}
  end
end
```
