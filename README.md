# uriel
Uriel is a NodeJS StatsD agent that pushes system, memory, cpu, network and disk usage statistics to any compatible StatsD (e.g. StatsD, Telegraf, DogStatsD) listener over UDP.

It can be run either as standalone server, or it can be embedded within another application.

## Embedded Runner

### Installation

```
$ npm install uriel
```

### Usage
```js
  const Uriel = require('uriel');

  let statsd = new Uriel(config, logger);
  statsd.init();
```

### Configuration
The configuration object which is passed to Uriel must contain the following (with defaults provided below):
```
{
  server: {
    shutdownTime: 1000,
    pollingTimer: 5000
  },
  statsd: {
    host: '127.0.0.1',
    port: '8125',
    name: 'Uriel',
    telegraf: false
  }
}
```

 * `server.pollingTime` -- the frequency of data pushes to the StatsD server.
 * `statsd.host` -- the server host where the StatsD server is running.
 * `statsd.port` -- the UDP port that the StatsD server is listening on.
 * `statsd.name` -- the `serverName` tag that is provided for all stats that are pushed. This allows info from differing servers to be distinguished from one another.
 * `statsd.telegraf` -- `true` or `false` value that specifies that the listening server is running telegraf

### Logging
The logger object supports passing any object which supports the `log`, `debug`, `info`, and `error` methods such as those provided by [Winston](https://www.npmjs.com/package/winston) or [Bunyan](https://www.npmjs.com/package/bunyan)

### Startup
To start the statsd agent the `.init()` method should be used.

### Shutdown
To shutdown the statsd agent the `.close()` method should be used.

## Standalone Server

### Installation
```
$ git clone https://github.com/chronosis/uriel
$ cd uriel
```

### Configuration
The file `config/config.js` should be edited to reflect any changes you may have. The standalone server uses [Winston](https://www.npmjs.com/package/winston) for logging and logs to the `logs/` folder.

### Running
#### With NodeJS
From the Uriel folder, perform:
```
$ node app.js
```

#### With PM2
```
$ pm2 start app.js -n "Uriel" -i 0
```

## StatsD Buckets
The following buckets are used to capture statistics:

### System
 * `system.uptime`
 * `system.load1`
 * `system.load5`
 * `system.load15`

### CPU
 * `cpu.usage_user`
 * `cpu.usage_nice`
 * `cpu.usage_system`
 * `cpu.usage_idle`
 * `cpu.usage_irq`

For all CPUs

### Memory
 * `mem.free`
 * `mem.total`
 * `mem.used`

### Network
 * `network.close_wait`
 * `network.estab`
 * `network.establised`
 * `network.last_ack`
 * `network.listen`
 * `network.syn_sent`
 * `network.time_wait`

### Disk Usage
 * `disk.free`
 * `disk.total`
 * `disk.used`

For all disks

## API

### constructor(config, logger)
```
const Uriel = require('uriel');

let statsd = new Uriel(config, logger);
```

Where the config parameter is the config object as outlined above, and the logger parameter is any compatible logging instance as outlined above. If no logger is provided, then all logging is sent to `/dev/null`

### .init()
Initializes and starts the Uriel statsD agent

### .close()
Shuts down the Uriel StatsD agent
