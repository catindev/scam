# Task Contract: <ID — название>

## Outcome

Наблюдаемый результат одним абзацем.

## Scope

### In scope

- ...

### Out of scope

- ...

## Allowed changes

| Repository/component | Paths | Permission |
| --- | --- | --- |
| ... | ... | read/write |

## Sources to read

- проектный entrypoint;
- ...

## Acceptance — frozen after first edit

- [ ] Поведение: ...
- [ ] Ошибка/граница: ...
- [ ] Совместимость: ...
- [ ] Evidence: команда `<command>` завершается с кодом 0.

## Owning gate

`focused | quick | package | integration | acceptance`

## External actions

Разрешены: ...

Запрещены без отдельного подтверждения: deploy, publish, push, production write,
credentials, destructive operations.

## Assumptions and risks

- `ASSUMPTION`: ...
- `RISK`: ...

## Stop conditions

- нужен продуктовый/архитектурный выбор;
- требуются новые полномочия;
- задача затрагивает неуказанную публичную границу;
- acceptance нельзя проверить в доступной среде.

