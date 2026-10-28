# QUESTORY ONLINE — GUIA DE PUBLICAÇÃO (USO PESSOAL)

## O que ESTE pacote faz
- Login exclusivo pelo e-mail em `OWNER_EMAIL` (autenticado via Supabase Auth).
- A IA cria uma rota com módulos e aulas a partir de assunto livre ou texto enviado.
- Extração de PDF/DOCX/TXT/MD/CSV, prática, prova RPG com HP, XP, ouro e loja com inventário.
- Exclusão de jornadas; persistência no Supabase; registro de acertos e revisões por tópico.

## 1 — Supabase
1. Crie projeto em https://supabase.com/dashboard.
2. Em **SQL Editor**, abra e execute **supabase/001_schema.sql** em um projeto NOVO (SQL cria tabelas; não serve para executar arquivos de site).
3. Em **Authentication > Users**, crie manualmente seu usuário com e-mail e senha; marque e-mail como confirmado se usar essa opção. Em **Authentication > Providers > Email**, desabilite a opção de novas inscrições (*Allow new users to sign up* / *Enable sign ups*, nomenclatura variável). Use uma senha forte.
4. Em **Project Settings > API Keys**, pegue a **Project URL**, uma chave **publishable** e uma chave **secret** (se seu projeto oferece apenas chaves legadas, use anon e service_role respectivamente, mas não exponha a service_role).
5. Segurança: políticas RLS incluídas permitem leitura privada de dados da conta. A chave secret/service_role fica SOMENTE no servidor, nunca no navegador ou GitHub.

## 2 — Preparar no computador
1. Instale Node.js 20+.
2. Extraia este zip, abra terminal nesta pasta e execute `npm install`.
3. Copie `.env.example` para `.env` e preencha com as chaves e seu e-mail. Nunca publique o `.env`.
4. Execute `npm start` e visite http://localhost:3030.
5. Faça login; gere uma jornada e depois cinco questões; teste prática e prova.

## 3 — Publicar online (Supabase NÃO hospeda este servidor Express)
1. Crie um repositório **privado** no GitHub e envie o projeto (NÃO envie `.env`).
2. Entre em https://dashboard.render.com e escolha **New > Web Service** conectando o repositório.
3. Runtime Node, Build Command `npm install`, Start Command `npm start`.
4. Em **Environment** defina SUPABASE_URL, SUPABASE_PUBLISHABLE_KEY, SUPABASE_SECRET_KEY, OWNER_EMAIL, OPENAI_API_KEY e OPENAI_MODEL. A chave secret e a da OpenAI nunca ficam no cliente.
5. Abra a URL `https://...onrender.com` que o Render fornecer. HTTPS é incluído.
6. Mantenha cadastro público desativado, guarde senhas e configure limites de gastos da OpenAI.

## Restrições importantes desta versão
- A IA **não** pesquisa automaticamente a web nem valida fontes atuais; ela usa o tema e material enviado.
- As questões são geradas ao pedir e armazenadas na nuvem; a rota inicial é gerada uma vez, não atualizada sozinha.
- Adaptação inicial é a dificuldade sugerida por índice de domínio; não há motor completo de replanejamento curricular ou chefes especiais persistentes.
- Itens/skins/reino têm economia inicial, mas **ainda não há equipamentos aplicáveis, construções visuais, consumo de poções ou animações de batalha**.
- O backend atual registra XP apenas ao ganhar prova; treino e prática não dão ouro/XP.
- A lógica de recompensa e de compras nesta entrega ainda NÃO usa transações atômicas em banco: mantenha o serviço privado de uso individual e evite múltiplas abas/requisições simultâneas. Antes de produção aberta, mover ações críticas para transações SQL/RPC.
- Como projeto de uso pessoal, não há recuperação automática de senha na interface. Redefina pelo painel Supabase se necessário.
- O modelo da API OpenAI é pago por utilização e independente do ChatGPT Plus.
- Não foi possível testar chamadas reais com suas credenciais; o deploy depende das configurações acima.
