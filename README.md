![Black and Red Modern Horror YouTube Thumbnail (3)](https://github.com/user-attachments/assets/b8f6ae77-0789-4a4c-9768-84f3f39a40a5)

Documentação da Aplicação de Transcrição de Áudio Descrição Aplicação web que converte arquivos de áudio em texto, gerando documentos em formato DOCX e PDF. Utiliza AssemblyAI para transcrição, Flask como backend, Celery para processamento assíncrono e Redis como message broker. Requisitos Python 3.9+ Redis Server Docker (opcional) Estrutura do Projeto transcricao-audio/ ├── static/ │ ├── fonts/ │ ├── style.css │ └── header_image.png ├── templates/ │ └── index.html ├── uploads/ ├── processed/ ├── app.py ├── tasks.py ├── requirements.txt ├── Dockerfile └── docker-compose.yml

Instalação Local Clone o repositório e crie um ambiente virtual: git clone [url-do-repositorio] cd transcricao-audio python -m venv venv source venv/bin/activate # Linux/Mac venv\Scripts\activate # Windows

Instale as dependências: pip install -r requirements.txt

Instal
e e inicie o Redis Server:

Ubuntu/Debian
sudo apt-get install redis-server sudo service redis-server start

Windows
Baixe e instale o Redis do site oficial
Crie as pastas necessárias: mkdir uploads processed

Configure a chave API do AssemblyAI: xxxxxxxxxxxxxxxxxxxxxxxxxxxx

Execução Local Inicie o worker do Celery: celery -A tasks worker --loglevel=info

Em outro terminal, inicie o Flask: python app.py

Acesse a aplicação em: http://localhost:3002 Execução com Docker Construa e inicie os containers: docker-compose up --build

Acesse a aplicação em: http://localhost:3002 Configuração em Servidor de Produção Configure o Nginx: server { listen 80; server_name seu-dominio.com;

location / {
    proxy_pass http://localhost:3002;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
}

Configure o Supervisor para gerenciar os processos: [program:transcricao_web] command=/caminho/para/venv/bin/python app.py directory=/caminho/para/aplicacao user=seu_usuario autostart=true autorestart=true

[program:transcricao_worker] command=/caminho/para/venv/bin/celery -A tasks worker --loglevel=info directory=/caminho/para/aplicacao user=seu_usuario autostart=true autorestart=true

Variáveis de Ambiente CELERY_BROKER_URL: URL do Redis (padrão: redis://localhost:6379/0) CELERY_RESULT_BACKEND: URL do Redis (padrão: redis://localhost:6379/0) ASSEMBLY_AI_KEY: Sua chave API do AssemblyAI Troubleshooting Problemas com Redis: Verifique se o Redis está rodando: redis-cli ping Verifique as conexões: redis-cli client list Problemas com Celery: Verifique os logs: celery -A tasks worker --loglevel=debug Limpe as tarefas pendentes: celery -A tasks purge Problemas com permissões: chmod 755 uploads processed chown -R seu_usuario:seu_usuario uploads processed

Segurança Configure HTTPS em produção Limite os tipos de arquivos aceitos Configure rate limiting Mantenha as dependências atualizadas
