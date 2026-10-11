# Manutencaoquedaroi — ManutençãoQueGeraRoi • Check / Check

Aplicação web/PWA de checklist para manutenção de colhedoras de cana, preparada para publicação em **GitHub Pages** e uso de **Supabase** como banco central.

## O que já está incluído

- John Deere CH950: 655 registros.
- John Deere CH570: 614 registros.
- Case IH 9900: 203 registros.
- Total: **1.472 registros**.
- Códigos OEM repetidos foram preservados.
- Busca por OEM, mangueira, conjunto, ST, item e conexões.
- Inspeção por frota/unidade, máquina e horímetro.
- Registro de pendências com condição, prioridade, responsável, prazo, status e fotos.
- Edição de inspeções e pendências.
- Relatório por frota.
- Relatório por máquina.
- Relatório gerencial.
- Impressão/Salvar como PDF pelo navegador.
- PWA para celular.
- SQL do banco Supabase.
- SQL de importação dos 1.472 registros.

## Arquitetura

**GitHub Pages** = publica o aplicativo.

**Supabase** = banco central, autenticação e armazenamento de fotos.

**Domínio** = `manutencaoquedaroi.com/check/`

GitHub sozinho não substitui o banco relacional para este projeto.

## Configuração do Supabase — passo a passo

1. Crie um projeto no Supabase com o nome sugerido:
   **Manutencaoquedaroi- Check**
2. Abra o **SQL Editor**.
3. Execute primeiro:
   `supabase/schema.sql`
4. Execute depois:
   `supabase/import_catalogos.sql`
5. Crie os usuários em **Authentication > Users**.
6. No aplicativo, abra ⚙ Configurações e informe:
   - Project URL
   - Publishable/anon key
7. **Nunca** coloque a `service_role`/secret key no GitHub ou no navegador.

## GitHub Pages

1. Crie um repositório, por exemplo:
   `Manutencaoquedaroi-Check`
2. Envie todos os arquivos desta pasta para a raiz do repositório.
3. Em **Settings > Pages**, selecione publicação pelo branch principal e pasta `/root`.
4. O site ficará disponível no endereço do GitHub Pages.
5. Depois configure o domínio `manutencaoquedaroi.com` para apontar para o GitHub Pages e mantenha o caminho `/check/`.

> Se o site for publicado diretamente na raiz do repositório, a aplicação precisa continuar sendo acessada em `/check/` conforme o `<base href="/check/">` atual. Se a hospedagem usar outro caminho, altere esse valor.

## Importação do catálogo

O arquivo `supabase/import_catalogos.sql` é a carga inicial do catálogo. Ele mantém registros repetidos quando o mesmo OEM aparece em aplicações diferentes.

## Dados locais e banco central

A versão entregue mantém o funcionamento local do aplicativo como fallback. A configuração do Supabase está preparada no banco e na tela de configurações. A próxima etapa de integração deve substituir o armazenamento local por leitura/gravação autenticada no Supabase, incluindo upload das fotos para o bucket privado `finding-photos`.

## Estrutura do banco

- `profiles`
- `fleets`
- `machines`
- `catalog_parts`
- `inspections`
- `findings`
- `finding_photos`
- `finding_history`

Relacionamento principal:

**Frota → Máquina → Inspeção → Pendência → Peça/OEM → Foto → Responsável → Status → Histórico**

## Fonte dos dados

Os dados do catálogo são derivados dos catálogos Copecar fornecidos pelo usuário. Campos ausentes permanecem vazios; não foram preenchidos por inferência.


## Usuários e login

Esta versão inclui autenticação por e-mail e senha via Supabase Auth.

1. Execute `supabase/schema.sql` no SQL Editor do projeto Supabase.
2. Publique os arquivos no GitHub Pages.
3. Abra `/check/`, informe a Project URL e a Publishable/anon key na tela de login.
4. Clique em **Criar usuário** ou entre com um usuário existente.
5. Para transformar um usuário em gestor, altere `public.profiles.role` para `gestor` ou `admin` no Supabase.

A chave usada no navegador deve ser somente a chave pública (publishable/anon). Nunca coloque a service role/secret key no GitHub ou no JavaScript.

### Observação
A autenticação já está preparada nesta versão. O próximo passo de produção é substituir o armazenamento local das inspeções/pendências pela persistência transacional no Supabase, permitindo que relatórios e pendências sejam compartilhados entre todos os usuários em tempo real.

## Fase 2 — persistência central implementada

A aplicação agora usa o Supabase como fonte central quando a conexão está configurada:

- autenticação por e-mail/senha;
- catálogo carregado do banco;
- criação automática de frota e máquina quando necessário;
- inspeções gravadas em `public.inspections`;
- pendências gravadas em `public.findings`;
- histórico de alteração de status/prioridade;
- fotos enviadas para o bucket privado `finding-photos`;
- relatórios alimentados pelos dados sincronizados do banco;
- fallback local somente quando o Supabase não estiver configurado.

### Teste recomendado antes da operação

1. Criar um usuário piloto.
2. Criar uma inspeção para uma máquina piloto.
3. Criar uma pendência com foto.
4. Sair e entrar novamente.
5. Confirmar que inspeção e pendência continuam disponíveis.
6. Alterar status/prioridade e confirmar o histórico.
7. Abrir o relatório gerencial.
8. Testar o acesso com um segundo usuário para confirmar as regras de segurança.

## Campos incluídos no PDF de pendências
O cabeçalho do relatório inclui empresa, frota/unidade, frente de trabalho, máquina(s), horímetro(s), data/hora de emissão e e-mail do usuário autenticado que gerou o relatório. Configure o nome da empresa em **Configurações → Dados do relatório**. A frente de trabalho é informada na inspeção e salva no Supabase.

### Atualização de banco já existente
Se o Supabase já estiver criado, execute `supabase/migration_work_front.sql` no SQL Editor antes de publicar esta versão. Para projeto novo, o `schema.sql` já contém a coluna `work_front`.
