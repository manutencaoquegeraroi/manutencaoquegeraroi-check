# Checklist de publicação — Manutencaoquedaroi / Check

## A. Supabase
- [ ] Criar projeto `Manutencaoquedaroi- Check`
- [ ] Executar `supabase/schema.sql`
- [ ] Executar `supabase/import_catalogos.sql`
- [ ] Criar usuários em Authentication > Users
- [ ] Conferir tabelas e os 1.472 registros em `catalog_parts`
- [ ] Não publicar service role key

## B. GitHub
- [ ] Criar repositório `Manutencaoquedaroi-Check`
- [ ] Enviar os arquivos da pasta
- [ ] Ativar GitHub Pages
- [ ] Publicar pelo branch principal
- [ ] Testar `/check/`

## C. Domínio
- [ ] Configurar GitHub Pages/custom domain
- [ ] Validar HTTPS
- [ ] Testar `manutencaoquedaroi.com/check/`

## D. Aplicativo
- [ ] Configurar Project URL + publishable/anon key no menu ⚙
- [ ] Testar catálogo CH950
- [ ] Testar catálogo CH570
- [ ] Testar Case 9900
- [ ] Criar uma inspeção
- [ ] Criar uma pendência
- [ ] Editar pendência
- [ ] Gerar relatório por frota
- [ ] Gerar relatório gerencial
- [ ] Salvar relatório como PDF
- [ ] Testar no iPhone

## E. Próxima integração recomendada
- [ ] Login no aplicativo via Supabase Auth
- [ ] Gravação das inspeções no Supabase
- [ ] Gravação das pendências no Supabase
- [ ] Upload das fotos no Storage
- [ ] Relatórios diretamente do banco
- [ ] Dashboard gerencial por unidade/frota/modelo
- [ ] Controle de perfis: operador, mecânico, gestor e administrador

- [ ] Testar criação de usuário e login
- [ ] Confirmar trigger de perfil em `public.profiles`
- [ ] Definir usuários gestores (`gestor`/`admin`)
- [ ] Nunca publicar service role/secret key

## Atualização do PDF (Fase 2.1)
- [ ] No Supabase existente, executar `supabase/migration_work_front.sql`.
- [ ] Publicar os arquivos atualizados do pacote.
- [ ] Em Configurações, preencher o nome da empresa e salvar.
- [ ] Ao criar/editar inspeção, preencher Frota/Unidade, Frente de trabalho, Máquina e Horímetro.
- [ ] Gerar PDF e conferir empresa, frota, frente, máquina, horímetro e e-mail de quem gerou.
