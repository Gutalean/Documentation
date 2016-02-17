# FAQ 


#### Должен ли порт 15441 быть открытым?

Не это __не обязательно__, Вы можете просматривать и пользоваться Зеросайтами с закрытым портом.
Однако, для создания новых сайтов мы крайне рекомендуем вам держать порт открытым.

После запуска ZeroNet пытается открыть порт на Вашем роутере, используя 
[UPnP](https://wikipedia.org/wiki/Universal_Plug_and_Play), но, в случае, если это не получилось, Вы можете попытаться открыть его вручную:

- Попробуйте запустить веб-интерфейс Вашего роутера в браузере по адресу:  [http://192.168.1.1](http://192.168.1.1)
или [http://192.168.0.1](http://192.168.0.1)
- Проверьте, стоит ли галочка напротив пункта "Enable UPnP support" (или аналогичного)

Если это не помогло, попытайтесь найти раздел с переброской портов "port forwarding". Он разный на каждом роутере. Здесь находится обучающий ролик на Youtube [Here is a tutorial on YouTube.](https://www.youtube.com/watch?v=aQXJ7sLSz14) Перебросить необходимо порт 15441.


---


#### Как в ZeroNet обстоят дела с анонимностью?

ZeroNet не более анонимен чем BitTorrent, но приватность (возможность выяснить кто является создателем конкретного сайта или автором отдельного комментария) возрастает с ростом сети и увеличением количеством пиров у каждого сайта.

ZeroNet подходит для работы с анонимными сетями: вы можете легко спрятать свой IP-адрес, используя сеть Tor.


---


#### Как использовать ZeroNet через Tor?

Если вы хотите спрятать свой IP, установите последнюю версию ZeroNet, а затем запустите его со следующим параметром: `zeronet.py --tor always`

На Windows нет необходимости в дополнительных настройках, так как Tor работает "из коробки". Для других ОС следуйте [инструкции по установке Tor](https://www.torproject.org/docs/installguide.html),
отредактируйте Ваш файл конфигурации torrc, убрав `#` из строки `# ControlPort 9051` а затем перезапустите сервис Tor и ZeroNet.

> __Совет:__ Вы можете проверить свой IP адрес на странице ZeroNet [статистики](http://127.0.0.1:43110/Stats).

> __Совет:__ Если у вас возникают ошибки при соединении с сетью убедитесь, что у Вас установлена последняя версия Tor (необходима версия не ниже 0.2.7.5)


---


#### Как заставить ZeroNet работать через Tor под Linux?

Обновите Tor до последней версии (не ниже 0.2.7.5), и следуйте этой [инструкции](https://www.torproject.org/docs/debian.html.en). Например, для Debian:

 - `echo 'deb http://deb.torproject.org/torproject.org jessie main'>> /etc/apt/sources.list.d/tor.list`
 - `gpg --keyserver keys.gnupg.net --recv 886DDD89`
 - `gpg --export A3C4F0F979CAA22CDBA8F512EE8CBC9E886DDD89 | apt-key add -`
 - `apt-get update`
 - `apt-get install tor`

Отредактируйте config для включения контроля протокола:
 
 - `mcedit /etc/tor/torrc`
 - Удалите символ `#` из строки `ControlPort 9051` и `CookieAuthentication 1` (строка ~57)
 - `/etc/init.d/tor restart`
 - Добавьте себе разрешение на чтение авторских cookie при помощи `usermod -a -G debian-tor [yourlinuxuser]`<br>(если вы не используете Debian проверьте группу пользователей файла командой `ls -al /var/run/tor/control.authcookie`)
 - Разлогиньтесь и залогиньтесь снова под своим пользователем для применения групповых изменений

> __Совет:__ Вы можете проверить корректность работы Tor командой `echo 'PROTOCOLINFO' | nc 127.0.0.1 9051`

> __Совет:__ Это также можно делать без изменения torrc (или используя старые версии клиента Tor) при помощи команды: `zeronet.py --tor disable --proxy 127.0.0.1:9050 --disable_udp`, но тогда у Вас не будет возможности связываться с другими адресами .onion.



---

#### Могу ли использовать одно и тоже имя пользователя (никнейм) на разных машинах?

Да, для этого достаточно скопировать на другую машину файл `data/users.json`.


---


#### How can I register a .bit domain?

You can register .bit domains using [Namecoin](https://namecoin.info/).
Manage your domains using the client's GUI or by the [command line interface](http://christopherpoole.github.io/registering-a-.bit-domain-with-namecoin/).

After the registration is done you have to edit your domain's record by adding a zeronet section to it, eg.:

```
{
...
    "zeronet": {
        "": "1EU1tbG9oC1A8jz2ouVwGZyQ5asrNsE4Vr",
        "blog": "1BLogC9LN4oPDcruNz3qo1ysa133E9AGg8",
        "talk": "1TaLk3zM7ZRskJvrh3ZNCDVGXvkJusPKQ"
    },
...
}
```
"" means the top domain, any other that that is a sub-domain.


> __Совет:__ You can buy Namecoin for Bitcoin or other cryptocurrencies using [shapeshift.io](https://shapeshift.io/).

> __Совет:__ Other possibilities to register .bit domains: [domaincoin.net](https://domaincoin.net/), [peername.com](https://peername.com/), [dotbit.me](https://dotbit.me/)

> __Совет:__ You can verify your domain on [namecha.in](http://namecha.in/), for example: [zeroid.bit](http://namecha.in/name/d/zeroid)

> __Совет:__ You should use only [lower-cased letters, numbers and - in your domains](http://wiki.namecoin.info/?title=Domain_Name_Specification_2.0#Valid_Domains).

> __Совет:__ To make ZeroHello to link your domain instead of your site's address, add a domain key to your content.json. ([Example](https://github.com/HelloZeroNet/ZeroBlog/blob/master/content.json#L6))


---


#### Can I use the generated site address/private key to accept Bitcoin payments?

Yes, it's a standard Bitcoin address. The private key is WIF formatted, so you can import it in most clients.

> __Совет:__ It's not recommended to keep a high amount of money on your site's address, because you have to enter your private key every time you modify your site.


---


#### What happens when someone hosts malicious content?

The ZeroNet sites are sandboxed, they have the same privileges as any other website you visit over the Internet.
You are in full control of what you are hosting. If you find suspicious content you can stop hosting the site at any time.


---


#### Is it possible to install ZeroNet to a remote machine?
Yes, you have to enable the UiPassword plugin by renaming the __plugins/disabled-UiPassword__ directory to __plugins/UiPassword__,
then start ZeroNet on the remote machine using <br>`zeronet.py --ui_ip "*" --ui_password anypassword`.
This will bind the ZeroNet UI webserver to all interfaces, but to keep it secure you can only access it by entering the given password.

> __Совет:__ You can also restrict the interface based on ip address by using `--ui_restrict ip1 ip2`.

> __Совет:__ You can specify the password in config file by creating a `zeronet.conf` file and add `[global]`, `ui_password = anypassword` lines to it.


---


#### Is there anyway to track the bandwidth ZeroNet is using?

The sent/received bytes are displayed at ZeroNet's sidebar.<br>(open it by dragging the topright `0` button to left)

> __Совет:__ Per connection statistics page: [http://127.0.0.1:43110/Stats](http://127.0.0.1:43110/Stats)


---


#### What happens if two people use the same keys to modify a site?

Every content.json file is timestamped, the clients always accepts the newest one.


---


#### Does ZeroNet uses Bitcoin's blockchain?

No, ZeroNet only using the cryptography of Bitcoin for site addresses and content signing/verification.
The users identification is also based on Bitcoin's [BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) format.

Namecoin's blockchain is being used for domain registrations.


---


#### Does ZeroNet only support HTML, CSS websites?

ZeroNet is built for dynamic, real-time updated websites, but you can serve any kind of files using it.
(VCS repositories, your own thin-client, database, etc.)


---


#### How can I create a new ZeroNet site?

[Follow those instructions.](/using_zeronet/create_new_site/)

---


#### How does it work?

- When you want to open a new site it asks visitors IP addresses from BitTorrent network
- First downloads a file named __content.json__, which holds all other filenames,
  __hashes__ and the site owner's cryptographic signature
- __Verifies__ the downloaded content.json file using the site's __address__ and the site owner's __signature__ from the file
- __Downloads other file__ (html, css, js...) and verifies them using the SHA512 hash fro content.json file
- Each visited site becomes __also served by you__.
- If the site owner (who has the private key for the site address) __modifies__ the site, then he/she signs
  the new content.json and __publishes it to the peers__. After the peers have verified the content.json
  integrity (using the signature), they __download the modified files__ and publish the new content to other peers.

More info:
 [Description of ZeroNet sample sites](/using_zeronet/sample_sites/),
 [Slides about how does ZeroNet work](https://docs.google.com/presentation/d/1_2qK1IuOKJ51pgBvllZ9Yu7Au2l551t3XBgyTSvilew/pub)

