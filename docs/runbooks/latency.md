Деградация приложения в K8s
Симптомы: Рост HTTP 5xx ошибок, задержки (latency > 2s), жалобы пользователей.
1. Быстрая диагностика
Проверьте статус подов и наличие перезапусков:

   kubectl get pods -n production

   Если RESTARTS > 0 или статус CrashLoopBackOff — сервис нестабилен.

2. Анализ логов
Найдите причину в последних 100 строках:

   kubectl logs -l app=my-app -n production --tail=100

3. Восстановление (safe restart)
Если логи не указывают на явную ошибку в БД/внешних API, выполните безопасный перезапуск пода (rolling update):

   kubectl rollout restart deployment/my-app -n production

4. Откат (rollback)
Если проблема началась сразу после деплоя, вернитесь к стабильной версии:

   kubectl rollout undo deployment/my-app -n production
