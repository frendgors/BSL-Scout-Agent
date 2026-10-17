# Исследование и источники

Проверено 2026-10-17. Здесь зафиксированы внешние факты; проектные гипотезы находятся в предложении архитектуры. Внешние цифры не являются нашими замерами.

## ColGrep и маленький encoder

- [LateOn-Code-edge](https://huggingface.co/lightonai/LateOn-Code-edge): семейство класса 17M параметров, multi-vector / ColBERT-style late interaction. Карточка показывает проекцию в 48-мерные векторы токенов и MaxSim. В языках указаны English/code; перенос на русские запросы к 1С нужно проверить.
- [LateOn-Code](https://huggingface.co/lightonai/LateOn-Code): более крупный вариант; в сравнительной таблице карточки указаны 149M. В [README ColGrep](https://github.com/lightonai/next-plaid/blob/main/colgrep/README.md) встречается 137M. Это расхождение источников: перед экспериментом фиксировать revision, конфигурацию и фактическое число параметров. Утверждение «в ColGrep encoder около 180M» не описывает используемый по умолчанию edge.
- [Конфигурация edge](https://huggingface.co/lightonai/LateOn-Code-edge/blob/main/config.json): ModernBertModel, hidden_size 256, 7 слоёв, vocab_size 50370. Это конфигурация backbone; она не заменяет подсчёт параметров всего retrieval pipeline.
- [Документация ColGrep](https://lightonai.github.io/next-plaid/colgrep.html): локальный CLI и гибридный поиск. В опубликованном списке языков нет BSL/.bsl/.os. Наличие XML не доказывает понимания структуры конфигураций 1С.
- [NextPlaid](https://github.com/lightonai/next-plaid): кандидат для изучения локального multi-vector индекса и интеграции. Решение о его использовании ещё не принято.
- [PyLate](https://github.com/lightonai/pylate): обучение и retrieval для late-interaction моделей; в README есть contrastive training, cached contrastive loss и distillation. Кандидат для экспериментов дообучения encoder.

В ColBERT-подобном поиске документ представлен последовательностью векторов. Для каждого вектора запроса находится лучшее совпадение среди векторов документа; оценки агрегируются. Такая схема даёт более детальное сопоставление, но память индекса зависит от числа сохранённых токенов и размерности вектора. Маленький encoder не гарантирует маленький индекс.

## Агент

[ColGrep-Agent-2B](https://huggingface.co/lightonai/colgrep-agent-2B) — локализация кода на базе MiniCPM5-2B, SFT + RL/GRPO, три инструмента (`colgrep`, `terminal`, `finish`) и бюджет десять ходов.

[Эксперимент авторов](https://huggingface.co/blog/lightonai/colgrep-agent): 52 вопроса по 13 репозиториям; сообщается экономия API-токенов 34,3%, наблюдаемая точность 80,8% против 83,3% у grep, время 21,6 с против 13,0 с. Это поиск кода, а не проверка полного исправления задач, не тест на BSL и не замер на нашей машине.

## Что изучать у сообщества 1С

| Источник | Полезная идея для изучения |
|---|---|
| [code-index-mcp](https://github.com/Regsorm/code-index-mcp) | BSL/метаданные, точечное чтение, графы и компактные ответы; Configurator и EDT |
| [rlm-tools-bsl](https://github.com/Dach-Coin/rlm-tools-bsl) | Пакетная обработка и фильтрация перед возвратом контекста основной модели |
| [1c-mcp-metacode](https://github.com/ROCTUP/1c-mcp-metacode) | Граф объектов, кода, форм и расширений; отдельная агентная подсистема |
| [1c-vector-search](https://github.com/KvitunAA/1c-vector-search) | Локальная композиция лексического/векторного поиска и графа |
| [USER2-1C-code](https://huggingface.co/PruhaNLP/USER2-1C-code) | Доменный dense encoder для русского вопроса и BSL — контрольная альтернатива multi-vector retrieval |
| [1C-Ebench](https://huggingface.co/datasets/PruhaNLP/1C-Ebench) и [1C-RB](https://github.com/PruhaNLP/1C-RB) | Открытые данные и оценка поиска отдельных фрагментов; не заменяют агентный тест целой конфигурации |
| [Сравнение на ERP УХ](https://infostart.ru/1c/articles/2802838/) | Практический пример контроля покрытия, расхода токенов и симметричности состава индексов |

Таблица фиксирует источники идей, не решение копировать код. При заимствовании реализации нужно сохранить происхождение и требования соответствующей лицензии. В initial commit сторонний код и веса не включены.

Dense bi-encoder нельзя механически подставить вместо late-interaction encoder в тот же индекс: представление и механизм подсчёта сходства различаются. Для честного сравнения им нужны соответствующие поисковые реализации и одинаковый корпус.
