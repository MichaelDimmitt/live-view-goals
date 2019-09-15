## live-view-goals

collab site... two clients going at once we see the communication. 
1) https://dennisbeatty.com/2019/04/24/how-to-create-a-todo-list-with-phoenix-liveview.html
2) https://github.com/dnsbty/live_view_todos

validation ... 
1) https://elixircasts.io/phoenix-live-view
2) 

talk explaining why live view can be better than elixir channels:
<br/>https://www.youtube.com/watch?time_continue=6&v=8xJzHq8ru0M

setup asdf the right way:
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
