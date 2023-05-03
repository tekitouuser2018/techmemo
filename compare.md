### 互換性など

- Goは公式で互換性などについて約束しているためプロダクトに採用しやすい
https://go.dev/doc/go1compat

-----------

### マルチスレッド

- Ruby(Rails): Thread.newでスレッド生成。pumaがマルチスレッド対応。https://tech-book.precena.co.jp/software/backend/ruby-on-rails/rails-process-and-thread
  - https://railsguides.jp/threading_and_code_execution.html
- Go: goroutineでスレッド生成。
- Node.js: worker_threadsでスレッド生成。https://nodejs.org/api/worker_threads.html

----

### 権限管理（認可）

- Railsではpunditというgemを使用し簡単に管理できる。https://github.com/varvet/pundit

- Goではcasbin? https://github.com/casbin/casbin
  - https://future-architect.github.io/articles/20221004a/
  - https://github.com/avelino/awesome-go#authentication-and-oauth

----
