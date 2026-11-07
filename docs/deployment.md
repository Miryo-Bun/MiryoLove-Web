# Предпубликационная проверка и переключение MiryoLove Web

**Проверено:** 7 ноября 2026 года
**Статус:** архивный отчёт о прежней deployment-копии, не инструкция к текущему production. Текущий document root — `/mnt/Manchild/Project/MiryoLove/MiryoLove Web`; `/mnt/Manchild/Project/MiryoLove/MiryoLove Web-deploy` ниже упоминается только как историческая копия. Не выполнять команды этого отчёта без новой проверки и отдельного разрешения.

## 1. Deployment-копия

Document root-кандидат:

`/mnt/Manchild/Project/MiryoLove/MiryoLove Web-deploy`

В копии 17 файлов: 10 HTML (девять страниц и 404), 3 изображения, CSS, JS, `robots.txt` и `sitemap.xml`. В deployment нет `.git`, `docs/`, скрытых файлов, симлинков, README, LICENSE, конфигураций разработки, баз данных и файлов с именами резервных копий. Поиск распространённых маркеров приватных ключей и API-токенов совпадений не дал; это эвристическая проверка, не гарантия отсутствия любых секретов.

Сверка файлов с исходным сайтом не обнаружила расхождений среди опубликованных файлов. Исключения ожидаемы: внутренние документы и исходные неиспользуемые изображения в deployment не копируются. Все 10 HTML-файлов, 184 локальные ссылки/ресурса, CSS URL, ссылки на изображения из JS, sitemap и `robots.txt` прошли проверку.

Публичные канонические URL:

| Страница | Канонический URL |
|---|---|
| Главная | `/` |
| MiryoLove AI | `/miryolove-ai/` |
| MiryoLove Server | `/miryolove-server/` |
| Git-сервис | `/git-service/` |
| Конфиденциальность | `/privacy-policy/` |
| Cookies | `/cookies/` |
| Условия использования | `/terms-of-use/` |
| Журнал обновлений | `/changelog/` |
| Философия | `/philosophy/` |
| Неизвестный путь | пользовательская 404 с HTTP 404 |

`/about/` и `/contacts/` не добавлялись: содержимое находится в секциях главной. Sitemap содержит девять канонических адресов. Публичная дорожная карта удалена; старые `/roadmap.html`, `/roadmap` и `/roadmap/` перенаправляются на `/`. В исходнике и deployment-копии поиск `MiryoBoard`, `miryoboard`, `board.miryolove.org`, а также поиск roadmap по публичным HTML/CSS/JS/XML/TXT совпадений не дал.

## 2. Активный Caddy и полный кандидат

Служба работает как `caddy:caddy` и запускается командой `/usr/bin/caddy run --config /etc/caddy/Caddyfile`.

На момент прежнего read-only аудита 7 ноября 2026 года `/etc/caddy/Caddyfile` указывал на историческую копию `MiryoLove Web-deploy`. Текущая конфигурация указывает на `/mnt/Manchild/Project/MiryoLove/MiryoLove Web`; значения и команды ниже относятся к прежнему аудиту.

Доступный только на loopback admin API Caddy (`127.0.0.1:2019`) ответил на GET `/config/` кодом 200. Загруженный runtime JSON подтвердил тот же root и только эти два hostname; reverse proxy handlers и configured access logging отсутствуют. На момент проверки disk-конфигурация и загруженный runtime согласованы по host/root. Предыдущая проверка сообщала о `/mnt/Manchild/Project/MiryoLove/Web/1`; текущая конфигурация файла имеет более позднее время изменения. Кто и каким способом изменил/перезагрузил конфигурацию, установить не удалось; время запуска процесса не показывает время последнего Caddy reload.

Подготовленный полный файл `../deploy/Caddyfile.candidate` относится к прежнему snapshot и содержит дополнительные site blocks `ai.miryolove.org` и `press.miryolove.org`, которых сейчас нет в активном файле/runtime. **Не применять этот файл в текущем виде.** Он также включает пока не активный access log. Перед любым staging кандидат нужно вручную сверить и пересобрать только по согласованному актуальному активному конфигу; эта работа не меняла Caddy-кандидаты.

Команды проверки, выполненные 7 ноября 2026 года:

```sh
caddy validate --config /etc/caddy/Caddyfile --adapter caddyfile
```

`caddy validate` для текущего `/etc/caddy/Caddyfile` вернул `Valid configuration`. Полный старый кандидат с output path `/var/log/caddy/miryolove-access.json` не валидировался непривилегированным пользователем: `permission denied`. Это ограничение доступа, не установленная синтаксическая ошибка.

Изолированная локальная копия main-site блока прошла validation и тестовый HTTP-запрос. В тестовый access-log JSON вошли только `ts`, статические `level`, `logger`, `msg` и искусственный `visitor_user_agent`. UNIX timestamp согласован с UTC-временем запроса. Тестовые URL/query, IP, Referer, Cookie, Authorization, другие headers, тело запроса и метрики ответа в запись не попали. Это подтверждает фильтр только на тестовой копии; production access log не включён, а права пользователя `caddy` на новый файл не проверялись.

Read-only production HTTP(S)-проверки подтвердили 200 для девяти canonical pages и локальных JS/CSS/sitemap/robots, 404 для неизвестного пути, TLS verification result 0, HTTP → HTTPS и `www` → apex. Искусственный query parameter сохранялся при HTTP→HTTPS и `www`→apex redirect. Он теряется при redirect clean path без `/`, старых `.html` URL и снятой roadmap URL. Эта особенность текущей публичной конфигурации зафиксирована; активный файл ради неё не изменялся.

## 3. Доступ Caddy к deployment

`namei -l` показал mode `755` на родительских каталогах и deployment-каталоге; все файлы deployment имеют mode `644`. Поскольку Caddy не является владельцем этих файлов и не состоит в группе `miryo_bun`, доступ возможен через разрешения `other`: проход `x` по каталогам и чтение `r` файлов. Владельцы и права не менялись.

Фактический тест `open/read` с effective UID пользователя `caddy` не запускался: повышение привилегий не запрашивалось. Администратор может отдельно подтвердить доступ без изменения прав:

```sh
sudo -u caddy test -x '/mnt/Manchild/Project/MiryoLove/MiryoLove Web-deploy'
sudo -u caddy test -r '/mnt/Manchild/Project/MiryoLove/MiryoLove Web-deploy/index.html'
```

Обе команды должны завершиться с кодом `0`. При отказе остановиться и согласовать решение о доступе; не менять права автоматически.

## 4. Резервная копия и staging-кандидат

Локальный снимок содержимого активного `/etc/caddy/Caddyfile` находится в session storage; его SHA-256 совпал с активным файлом при последнем сравнении. Это не административная резервная копия и не сохраняет системные владельца/права. В `/etc/caddy` резервная копия не создавалась.

Текущий `../deploy/Caddyfile.candidate` **не является допустимым staged-кандидатом**: он содержит дополнительные site blocks, отсутствующие в текущем production-конфиге, и не валидируется обычным пользователем с фактическим `/var/log/caddy` output path. Не копировать и не применять его. Сначала получить отдельное разрешение и подготовить новый, согласованный кандидат, содержащий только нужные изменения.

Только после этого администратор может создать уникальную резервную копию:

```sh
backup="/etc/caddy/Caddyfile.pre-miryolove-$(date +%Y%m%d-%H%M%S)"
sudo test ! -e "$backup" || { echo "Backup path already exists; choose a new timestamp" >&2; exit 1; }
sudo cp -a -- /etc/caddy/Caddyfile "$backup"
sudo cmp -s /etc/caddy/Caddyfile "$backup"
sudo stat -c '%A %a %U:%G %n' "$backup"
```

После создания новой проверенной конфигурации staging должен использовать именно этот файл, а не старый `deploy/Caddyfile.candidate`:

```sh
sudo test ! -e /etc/caddy/Caddyfile.next || { echo "Staging path already exists; stop and inspect it" >&2; exit 1; }
sudo cp -a -- /etc/caddy/Caddyfile /etc/caddy/Caddyfile.next
sudo tee /etc/caddy/Caddyfile.next < /path/to/independently-reviewed-candidate >/dev/null
sudo cmp -s /path/to/independently-reviewed-candidate /etc/caddy/Caddyfile.next
sudo -u caddy test -w /var/log/caddy
sudo -u caddy caddy validate --config /etc/caddy/Caddyfile.next --adapter caddyfile
```

`/path/to/independently-reviewed-candidate` — заполнить реальным путём только после отдельного решения. Если кандидат содержит новый file log, validation от `caddy` может создать этот журнал; сначала согласовать это действие. При существующем `Caddyfile.next` остановиться и проверить его, не перезаписывать.

## 5. Контролируемое переключение (только после отдельного разрешения)

Эта процедура здесь не выполнялась. Не выполнять её для текущего старого кандидата. После новой подготовки, проверки резервной копии и отдельного разрешения:

```sh
sudo mv -- /etc/caddy/Caddyfile.next /etc/caddy/Caddyfile
sudo caddy validate --config /etc/caddy/Caddyfile --adapter caddyfile
```

Ожидаемый результат — `Valid configuration`. Если проверка не проходит, **не выполнять reload**; восстановить прежнюю конфигурацию по процедуре отката ниже. Только после успешной проверки и отдельного разрешения на применение:

```sh
sudo systemctl reload caddy
```

Reload ожидается успешным без остановки службы. Эта команда не запускалась.

После reload проверить HTTPS снаружи, с проверкой сертификата curl по умолчанию:

```sh
curl -sS -o /dev/null -w '%{http_code} %{url_effective}\n' https://miryolove.org/
curl -sS -o /dev/null -w '%{http_code} %{url_effective}\n' https://www.miryolove.org/miryolove-ai/?check=1
```

Ожидания: главная — 200; `www` — 301 на HTTPS apex с тем же путём/query. Повторить `curl -sS -L --max-redirs 5 -o /dev/null -w '%{http_code} %{num_redirects} %{url_effective}\n' "https://miryolove.org$path"` для `/`, каждого clean URL из таблицы, старых адресов, URL без `/`, а также:

```sh
curl -sS -o /tmp/miryolove-404-body -w '%{http_code}\n' https://miryolove.org/path-that-does-not-exist
grep -q '404' /tmp/miryolove-404-body
```

Для unknown URL ожидается статус `404` (не 200) и содержимое пользовательской страницы. После проверок удалить только созданный `/tmp/miryolove-404-body`. Если HTTPS-проверка не проходит, не менять DNS; собрать диагностические сведения и остановиться.

## 6. Откат

Откат требует отдельного решения администратора. Использовать только резервную копию, созданную на шаге 4; не заменять её и не выбирать один из существовавших backup-файлов без проверки:

```sh
sudo test ! -e /etc/caddy/Caddyfile.restore || { echo "Restore staging path already exists; stop and inspect it" >&2; exit 1; }
sudo cp -a -- "$backup" /etc/caddy/Caddyfile.restore
sudo caddy validate --config /etc/caddy/Caddyfile.restore --adapter caddyfile
```

После успешной проверки и отдельного разрешения:

```sh
sudo mv -- /etc/caddy/Caddyfile.restore /etc/caddy/Caddyfile
sudo caddy validate --config /etc/caddy/Caddyfile --adapter caddyfile
sudo systemctl reload caddy
```

Если восстановленная конфигурация не валидируется, не выполнять reload и оставить службу на её уже загруженной конфигурации, пока администратор не разберёт ошибку.

## 7. Итог доступности

На момент архивной проверки публичный домен обслуживался конфигурацией с root deployment-копии. Это утверждение не описывает текущий root; доступный текущий Caddyfile указывает на исходный каталог сайта. Полный подготовленный кандидат не является описанием текущей конфигурации и **не готов к применению**.

В этой сессии не менялись активный Caddyfile, production root, DNS, службы, журналы, права, владельцы, базы данных или snapshots. Нет разрешения на reload/restart или применение кандидата.
