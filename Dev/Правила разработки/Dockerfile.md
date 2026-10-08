
1. Первая строчка `# syntax=docker/dockerfile:1`
2. Если приложению в контейнере не нужны права root (например, открывать порты ниже 1024), то всегда запускаем под простым пользователем, типа app
3. Использовать multistage build, зависимости меняются редко, код часто. Т.е. в одной стадии устанавливаем все зависимости, во вторую делаем папке с зависимостями:

   ```Dockerfile
   COPY --link --from=builder /deps /deps
   ```

4. Правильная установка зависимостей с кешированием на уровне Docker layer. Для npm/deno/bun аналогично (где `$HOME` - путь к домашней директории, пользователя внутри контейнера, под которым устанавливаются зависимости):

   ```Dockerfile
   RUN --mount=type=cache,target=$HOME/.cache/pip\
       --mount=type=bind,source=requirements.txt,target=requirements.txt \
       pip install -r requirements.txt
   ```

5. Правильная установка дополнительных пакетов для ОС. Пример для Debian-like, для других пакетных менеджеров аналогично:

   ```Dockerfile
   RUN --mount=type=cache,target=/var/cache/apt,sharing=locked \
      --mount=type=cache,target=/var/lib/apt,sharing=locked
      apt install -y curl
   ```

6. Если ЯП в процессе сборки производит бинарники или скомпилированные/минимизированные (TypeScript, CSS и т.п.) файлы, то папка с исходниками в образ не копируется, а монтируется. Пример для Go:

   ```Dockerfile
   FROM golang:latest
   WORKDIR /build
   RUN --mount=type=bind,target=. go build -o /app/hello
   ```

7. Стараемся всегда указывать `HEALTHCECK`
   
   ```Dockerfile
   HEALTHCHECK --interval=15s --timeout=3s --retries=5 \
     CMD wget -q --spider http://127.0.0.1:8080/ || exit 1
   ```

7. Всегда указываем `EXPOSE` и `VOLUMES`, если наружу торчат порты и/или используются volume
8. Проверяем Dockerfile линтером [hadolint](https://github.com/hadolint/hadolint)