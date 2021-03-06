# Freecoin - digital social currency toolkit

[![software by Dyne.org](https://www.dyne.org/wp-content/uploads/2015/12/software_by_dyne.png)](http://www.dyne.org)

Freecoin aims to be a framework for remuneration and authentication supporting multi-sig and off-line transactions on top of multiple blockchain backends. It is open source, written in Clojure and comprising of a REST API and a clean user interface. Freecoin's main use-case is that of developing "social wallets" where balances and transactions are trasparent to entire groups of people to help participatory budgeting activities and organisational awareness.

[![Build Status](https://travis-ci.org/PIENews/freecoin.svg?branch=master)](https://travis-ci.org/PIENews/freecoin)

[![Code Climate](https://codeclimate.com/github/PIENews/freecoin.png)](https://codeclimate.com/github/PIENews/freecoin)

## Design

The design of Freecoin is informed by an extensive economic and user-centered research conducted by the D-CENT project and documented in deliverables that are available to the public:

- [Design of Social Digital Currency (D4.4)](http://dcentproject.eu/wp-content/uploads/2015/10/design_of_social_digital_currency_publication.pdf)
- [Implementation of digital social currency infrastructure (D5.5)](http://dcentproject.eu/wp-content/uploads/2015/10/D5.5-Implementation-of-digital-social-currency-infrastructure-.pdf).

More resources can be found on the D-CENT webpage: http://dcentproject.eu/resource_category/publications/

Furthermore, Freecoin's first social wallet pilots are informed by the research made in the [PIE Project](http://pieproject.eu).

## Configuration

Freecoin identity management is delegated to Stonecutter, the D-CENT SSO. To run Freecoin one also needs to configure integration with a running instance of Stonecutter configured to accept the Freecoin application. The configuration locations are:

- Freecoin: `profiles.clj`
- Stonecutter: `resources/client-credentials.yml`

This dependency may be removed in the close future as Stonecutter has been left unmaintained.

## Running the app inside a Vagrant virtual machine

Install the **latest** version of Vagrant and VirtualboxISO (be warned, most distributions have outdated packages which won't function well)

Then go into the `ops/` directory in Freecoin and run `vagrant up`, this will create and provision a new virtual machine running Freecoin.

Inside `ops/stonecutter` there is another setup to create and run a local Stonecutter SSO with the same command `vagrant up` given inside it. Once the Stonecutter SSO box is up and running, you should `vagrant ssh` into it and:

- edit the `/home/vagrant/stonecutter/resources/client-credentials.yml` to add the Freecoin client ID and secret in your `project.clj`
- run the application with `/home/vagrant/stonecutter/start_app_vm.sh`
- verify that Stonecutter is up and running at `http://localhost:5000`

## Running the app locally

Install all necessary dependencies, for instance using the following packages found on APT based systems:

```
openjdk-7-jdk mongodb libversioneer-clojure haveged mongodb-server
```

then install Leiningen which will take care of all Clojure dependencies

```
mkdir ~/bin
wget https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein -O ~/bin/lein
chmod +x ~/bin/lein
```

then start the MongoDB server in which Freecoin will store its data:

```
sudo service mongod start
```

then from inside the Freecoin source, start it with

```
lein ring server
```

This command will open a browser on localhost port 8000

### Running the app using the uberjar

- For freecoin
 java -cp target/uberjar/freecoin-<VERSION>-standalone.jar freecoin.main

## Running the app from a live repl (for developers)

The server can be started and stopped from the repl by doing the following

```
$ lein repl
freecoin.core=> (start) ;; starts the server
freecoin.core=> (stop) ;; stops the server
freecoin.core=> (use 'freecoin.handlers.debug :reload) (stop) (start) ;; refresh specific namespaces
```

## Live reloading of .clj modules in the repl

Every time you change a file, the tracker will reload it in the
running VM and show a message in the corner of your screen (using
`notify-send`; Linux only for now):

```
lein repl
user=> (use 'freecoin.dev)
user=> (start-nstracker) ;; starts the file change tracker
```


## Running the tests

Freecoin comes complete with test units which are run by the CI but can also be run locally.

For the purpose we use Clojure's `midje` package, to be run with:

```
lein midje
```

See: https://github.com/marick/Midje/wiki/A-tutorial-introduction for advanced testing features.


## License

This Free and Open Source research and development activity is funded by the [European Commission in the context of Collective Awareness Platforms for Sustainability and Social Innovation (CAPSSI)](https://ec.europa.eu/digital-single-market/en/collective-awareness) grants nr.610349 and nr.687922.

The Freecoin toolkit is Copyright (C) 2015-2017 by the Dyne.org Foundation, Amsterdam

Freecoin development is lead by Aspasia Beneti <aspra@dyne.org>

Freecoin co-design is lead by Denis Roio <jaromil@dyne.org> and Marco Sachy <radium@dyne.org>

With expert contributions by Carlo Sciolla, Duncan Mortimer, Arjan Scherpenisse, Amy Welch, Gareth Rogers and Joonas Pekkanen.

```
This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <http://www.gnu.org/licenses/>.
```
