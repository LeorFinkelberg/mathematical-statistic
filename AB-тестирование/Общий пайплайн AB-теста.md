### Пайплайн A/B-теста

1. Выбор метрики и ожидаемого эффекта. В качестве метрики для онлайн-кинотиатра, например, можно выбрать количество просмотров. То есть новый алгоритм рекомендаций внедряется только в том случае, если количество просмотров увеличивается не менее, чем на 3%.
2. Подбор критерия (параметрический, непараметрический и пр.). 
3. Подбор похожих групп. Нужно сделать так, чтобы группы A и B были похожи (то есть нужно сохранить баланс девочек и мальчиков, похожее распределение доходов и пр.).
4. Проводим A/A-тест на _оффлайн данных_. Оцениваем корректность -- то есть ищем процент случаев, когда мы нашли эффект, а его на самом деле нет. Этот процент не должен превышать $\alpha$. Например, не слишком ли часто наблюдаются ложные срабатывания (алгоритм говорит, что увлечение есть, а его на самом деле нет).
5. Оцениваем мощность пайплайна A/B-теста. То есть в каком проценте случаев удается поймать эффект, когда он на самом деле есть. И пытаемся понять как долго держать A/B-тест.

Для оценки корректности (A/A-тест) вычисляем количество пар, в которых $\text{p-value} < \alpha$ : то есть считаем в каком проценте случаев критерий (сработал) нашел различия, когда их на самом деле нет.

Процент пар ==не должен превышать уровень значимости== $\alpha$  (0.01, 0.05 и т.д.), а распределение ==p-value==, которое мы получили после A/A-теста на исторических данных, ==должно быть равномерным==. Без этого наши выводы будут смещены и потеряют смысл.

Для оценки мощности (проводится на оффлайн-данных) нужно подсчитать процент случаев, когда $\text{p-value} < \alpha$ : то есть нужно посчитать в каком проценте случаев критерий сработал или нашел различия, когда оно на самом деле есть.

### Оффлайн и онлайн метрики

Есть подход, который называется "исследователи в бою". Смотрим какие-то метрики (например, MAP@k, Presicion@k, nDCG etc.). Какие-то оффлайн метрики не упали, какие-то выросли и нам модель в целом нравится, поэтому мы ее раскатываем и надеемся на увеличение онлайн метрик.

Другой подход "осторожные исследователи". Пытаемся найти корреляции / построить модель, которая бы по оффлайн метрикам предсказывала бы онлайн метрики. Самое сложное -- это собрать датасет. Чем здесь помочь? Нужно логировать оффлайн и онлайн метрики на A/B-тесте. Здесь оффлайн метрики (Precision@k, nDCG etc.) будут признаками, а онлайн метрика (CTR, количество просмотров etc.) будет целью.

NB: рост оффлайн метрик не всегда означает увеличение онлайн метрик.

### Feedback Loop 

Нужно бороться с "пузырем" (feedback loop)!

Простые популярные решения:
- Случайно подмешивать рекомендации из популярного + эвристики
- Тестировать бандитов
- Глобальная контрольная группа без экспериментов
- Эвристики

Сложные решения:
- https://arxiv.org/pdf/2007.13019.pdf
- https://arxiv.org/pdf/1902.10730.pdf