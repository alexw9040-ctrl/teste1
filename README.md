# SIGCEM — Sistema Integrado de Gestão de Cemitérios

Sistema web, em português, para a administração de cemitérios municipais: cadastro de falecidos, sepulturas e
responsáveis, sepultamentos com validação de disponibilidade, exclusão lógica com restauração, auditoria
imutável e controle de acesso por perfil.

> **Status: versão 0.1 (Etapa 1).** Veja a tabela abaixo para o que já funciona e o que ainda não foi feito.
> Não utilize com dados reais antes de concluir as etapas de backup/restauração e revisão de segurança.

## O que está implementado (Etapa 1)

| Área | Situação |
|---|---|
| Login, logout, bloqueio por tentativas, sessões com invalidação | Implementado |
| Perfis (admin geral, admin do cemitério, operador, consulta) + permissões por módulo/operação | Implementado |
| Reautenticação para operações críticas (exclusão em lote, alteração de permissões) | Implementado |
| Falecidos, sepulturas, responsáveis: criar, editar, pesquisar, paginar | Implementado |
| Exclusão lógica, exclusão em lote (com justificativa), restauração, lista de excluídos | Implementado |
| Sepultamentos com protocolo, validação de capacidade/situação e integridade | Implementado |
| Auditoria append-only (CPF/RG mascarados) | Implementado |
| Painel com indicadores e gráfico mensal | Implementado (parcial) |
| Migração Alembic, Docker Compose, CLI para criar o administrador | Implementado |
| Interface React: login, painel e listas (somente leitura) | Esqueleto |

## Ainda não implementado

Exumações, transferências, upload de documentos, manutenção, mapa visual, relatórios PDF/XLSX, documentos
oficiais, backup/restauração, reinicialização administrativa, exclusão definitiva, formulários de edição no
frontend, modo offline/sincronização. Estão previstos para as próximas etapas (ver `docs/MANUAL.md`).

## Execução rápida (Docker Compose)

```bash
cp .env.example .env            # preencha POSTGRES_PASSWORD e SECRET_KEY
docker compose up -d --build
docker compose run --rm backend python -m app.cli create-admin   # pede a senha; não há senha padrão
```

Acesse `http://localhost:8080`. Em produção, coloque um proxy HTTPS na frente. Detalhes em `docs/INSTALL.md`.

## Desenvolvimento local

```bash
# Backend (Python 3.12)
cd backend && python -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
alembic upgrade head
python -m app.cli create-admin
uvicorn app.main:app --reload        # API em http://localhost:8000/api/docs

# Frontend
cd frontend && npm install && npm run dev   # http://localhost:5173

# Testes (na raiz)
pytest
```

## Estrutura

`backend/` API FastAPI · `frontend/` React + TypeScript + Tailwind · `database/` migrações e seeds ·
`storage/` e `backups/` dados locais (ignorados pelo Git) · `docs/` documentação · `tests/` testes pytest.

## Segurança e LGPD

Senhas com Argon2id; controle de acesso aplicado no backend; tokens de sessão curtos; auditoria imutável;
nenhum segredo ou dado real no repositório. Veja `docs/DATABASE.md` e `docs/BACKUP.md`. O tratamento de dados
pessoais de falecidos e familiares deve seguir a política do município e a Lei nº 13.709/2018.

## Licença

MIT (ver `LICENSE`). Ajuste o titular e a licença conforme a decisão do município antes de publicar.
