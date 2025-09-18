# Momo-store

## Как запустить проект

```bash
git clone https://github.com/Eduard-Bodreev/cloud-services-engineer-docker-project-sem2.git
cd cloud-services-engineer-docker-project-sem2
docker compose up -d
```
## Проверка работоспособности
### 1. Проверка фронтенда

Редирект с корня `/` на `/momo-store/`:
```bash
curl -I http://localhost/
```
Ожидаемый результат:
```
HTTP/1.1 301 Moved Permanently
Location: http://localhost/momo-store/
```
### 2. Проверка приложения
```bash
curl -I http://localhost/momo-store/
```
Ожидаемый результат:
```
HTTP/1.1 200 OK
Content-Type: text/html
```
### 3. Проверка API через фронтовый Nginx
```bash
curl -i http://localhost/api/health
```
Ожидаемый результат:
```
HTTP/1.1 200 OK
```
### 4. Проверка API напрямую
```bash
curl -i http://localhost:8081/health
```
Ожидаемый результат:
```
HTTP/1.1 200 OK
```

### Размер образов

**backend**: \~24.7 MB
**frontend**: \~50 MB

Образы получились компактными благодаря multi-stage builds.
Для бэкенда в финальный образ попадает только скомпилированный бинарник и минимальный runtime на базе alpine. Для фронтенда также используется alpine образ, в который копируются только статические файлы после сборки. В процессе сборки зависимости кэшируются, поэтому пересборка выполняется быстрее при неизменном коде.

### Безопасность

Используются облегчённые базовые образы alpine и nginx-unprivileged, что снижает поверхность атак. Финальные образы не содержат инструментов разработки, только то, что нужно для запуска приложений. В workflow настроен Trivy Security Scan, который проверяет образы на наличие уязвимостей. При обнаружении критических уязвимостей версии базовых образов обновляются.
В контейнерах сервисы работают от непривилегированного пользователя, файловая система монтируется в режиме read-only, права и capabilities урезаны.

