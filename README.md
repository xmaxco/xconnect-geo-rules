# xconnect-geo-rules

Минимальный slim-набор GeoIP/GeoSite правил для VPN-клиентов **Xconnect VPN**. Содержит только категории, релевантные для российского трафика и блокировки рекламы. Размер итоговых `.dat` файлов в **~12 раз меньше** оригинальных полных наборов.

Файлы публикуются в [Releases](../../releases/latest) и обновляются автоматически каждые 6 часов через GitHub Actions.

## Скачать последнюю версию

```
https://github.com/xmaxco/xconnect-geo-rules/releases/latest/download/xconnect-geoip.dat
https://github.com/xmaxco/xconnect-geo-rules/releases/latest/download/xconnect-geosite.dat
```

## Размеры

| Файл | Размер | Сжатие vs оригинал |
|------|-------:|-------------------:|
| `xconnect-geoip.dat`   | ~1.75 MB | -91% (было 19.67 MB) |
| `xconnect-geosite.dat` | ~5.42 MB | -92% (было 65.33 MB) |
| **Итого**              | **~7.2 MB** | **-92%** (было 85 MB) |

## Что внутри

### `xconnect-geoip.dat`

| Категория | CIDRs | Назначение |
|-----------|------:|-----------|
| `RU` | ~24 600 | Российские IP-сети для маршрутизации direct |
| `RU-BLOCKED` | ~88 500 | IP заблокированных в РФ ресурсов (antifilter.download) |
| `RU-BLOCKED-COMMUNITY` | ~900 | Народные дополнения (community.antifilter.download) |
| `RU-WHITELIST` | ~30 200 | Белый список РФ |
| `RE-FILTER` | ~25 000 | Альтернативный реестр re:filter |
| `YANDEX` | ~50 | IP-сети Yandex |
| `PRIVATE` | 18 | RFC1918, link-local, multicast |

### `xconnect-geosite.dat`

**RU-категории (для direct-маршрутизации):**

| Категория | Доменов | Что |
|-----------|--------:|-----|
| `category-ru` | ~915 | Базовый список российских доменов |
| `category-bank-ru` | ~307 | Российские банки (Сбер, Тинькофф, Альфа и т.д.) |
| `category-gov-ru` | ~128 | Госуслуги, ведомства |
| `category-ecommerce-ru` | ~175 | Маркетплейсы (Avito, Ozon и т.д.) |
| `category-retail-ru` | ~78 | Ритейл (М.Видео, Ашан, Бристоль и т.д.) |
| `category-medicine-ru` | ~20 | Медучреждения, ЕМИАС |
| `category-travel-ru` | ~24 | Транспорт (РЖД, метро, ВТБ-транспорт) |
| `category-entertainment-ru` | ~28 | Кинопоиск, Rutube, trbcdn |
| `category-betting-ru` | ~13 | Букмекеры |
| `category-media-ru` | ~4 | Российские СМИ |
| `category-media-ru-blocked` | ~131 | Заблокированные в РФ СМИ |
| `mailru` / `mailru-group` | ~297 | Mail.ru Group / VK ecosystem |
| `mts-ru` | ~12 | МТС |
| `myoffice-ru`, `genotek-ru`, `ideco-ru` | <15 | Прочие российские сервисы |
| `ru-available-only-inside` | ~153 | Сервисы доступные ТОЛЬКО изнутри РФ |
| `ru-blocked` | ~74 700 | Заблокированные в РФ домены (antifilter + re:filter) |

**Сервисные категории:**

| Категория | Доменов | Что |
|-----------|--------:|-----|
| `category-ads-all` | ~158 500 | Блокировка рекламы и трекеров |
| `private` | ~131 | RFC1918, локальные домены |

**Не включено** (по сравнению с оригиналом):
- `geosite:ru-blocked-all` (1.26M доменов) — содержит мусор, false positives и слишком общие правила, на практике избыточен поверх `ru-blocked` + дефолтной маршрутизации через прокси
- Все мировые категории (`geosite:cn`, `geosite:apple`, `geosite:microsoft`, `geolocation-!cn` и т.д.) — нерелевантны для нашего use case

## Атрибуция

Все исходные данные взяты из проекта [**runetfreedom**](https://github.com/runetfreedom) — большое спасибо автору за регулярное обновление списков 🙏

Конкретные источники (обновляются каждые 6 часов):
- [runetfreedom/russia-blocked-geoip](https://github.com/runetfreedom/russia-blocked-geoip) — `geoip.dat`
- [runetfreedom/russia-blocked-geosite](https://github.com/runetfreedom/russia-blocked-geosite) — `geosite.dat`
- [runetfreedom/russia-v2ray-rules-dat](https://github.com/runetfreedom/russia-v2ray-rules-dat) — комбинированный набор

Этот репозиторий **не заменяет** оригинальные проекты — он выполняет только фильтрацию по релевантным категориям. Если нужен полный набор правил, используйте оригинальные репозитории runetfreedom.

## Как собрать локально

Положите рядом `geoip.dat` и `geosite.dat` (от runetfreedom) и запустите:

```bash
python3 build_slim.py
```

На выходе появятся `xconnect-geoip.dat` и `xconnect-geosite.dat`.

## Лицензия

[GPL-3.0](LICENSE) — наследована от исходных проектов runetfreedom.
