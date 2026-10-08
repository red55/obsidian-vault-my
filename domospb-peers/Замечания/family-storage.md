
1. URL к микросервисам должны быть конфигурируемыми, а не зашитыми в код. Пример [backend/api_gateway/app/aggregator.py · main · peers / self-hosted-storage / self-hosted · GitLab](https://gitlab.apps.yunqi.studio/peers/self-hosted-storage/self-hosted/-/blob/main/backend/api_gateway/app/aggregator.py?ref_type=heads#L160)
2. Вычистить из исходников мусор (добавить в .gitignore):
	1. .DS_Store
	2. ___pycache/*
	3.  e.t.c.
3. Нет compose.yaml, как запустить?
4. 