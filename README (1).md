# Node.js App

تطبيق Express بسيط، متعمل له CI/CD باستخدام Jenkins وDocker.

## الـ Endpoints
- `GET /` : رسالة ترحيب
- `GET /health` : فحص الحالة

## التشغيل المحلي
```bash
npm install
npm start
```

## التشغيل بـ Docker
```bash
docker build -t node-app .
docker run -d -p 3000:3000 node-app
```
افتحي `http://localhost:3000`

## الـ CI/CD (Jenkins)
الـ `Jenkinsfile` بيعمل الخطوات دي:
1. Clone للريبو
2. Build لـ Docker image
3. Push للـ image على Docker Hub
4. Run للكونتينر
5. Smoke test على `/health`
