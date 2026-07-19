# Task Contract: <ID — название>

## Intake

- Type: `DESIGN | DIAGNOSIS | IMPLEMENTATION | REVIEW | OPERATION | CONTINUATION`
- Status: `DRAFT | FROZEN`
- User intent: ...
- Interpretation approved: `<reference or NOT_YET>`

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

## Acceptance — frozen after approval and before first product edit

- [ ] Поведение: ...
- [ ] Ошибка/граница: ...
- [ ] Совместимость: ...
- [ ] Evidence: команда `<command>` завершается с кодом 0.

Для `DESIGN`, `DIAGNOSIS`, `REVIEW`, `OPERATION` и `CONTINUATION`
используй type-specific acceptance из `INTAKE.md`; не имитируй реализацию
универсальными пунктами выше.

## Owning gate

`focused | quick | package | integration | acceptance | operation`

Почему этот уровень закрывает Outcome: ...

## External actions

Разрешены: ...

Запрещены без отдельного подтверждения: deploy, publish, push, production write,
credentials, destructive operations.

## Assumptions and risks

- `ASSUMPTION`: ...
- `RISK`: ...

## Proposed split

`NONE` или упорядоченные последующие Task Contracts, не входящие в текущий
acceptance:

1. ...

## Stop conditions

- нужен продуктовый/архитектурный выбор;
- требуются новые полномочия;
- задача затрагивает неуказанную публичную границу;
- acceptance нельзя проверить в доступной среде.
