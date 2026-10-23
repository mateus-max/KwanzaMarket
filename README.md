# KwanzaMarket 🇦🇴

**O mercado de Angola, online.** Marketplace para produtos físicos e digitais, com interface adaptável e instalável como PWA.

## O que está incluído nesta primeira versão

- Página inicial, catálogo, pesquisa, filtros por categoria/província e separação entre produtos físicos e digitais.
- Registo e login com e-mail/palavra-passe e Google via Firebase Authentication.
- Perfis de compradores e vendedores; candidatura de vendedor com estado pendente.
- Área de conta, carrinho, encomendas e submissão de comprovativo para análise.
- Área do vendedor para submeter produtos para aprovação.
- Painel administrativo para analisar vendedores, publicar/rejeitar produtos e aprovar/rejeitar comprovativos e encomendas.
- PWA básica, layout responsivo e moeda AOA/Kz.
- Firebase Realtime Database ligado ao projeto `kwanzamarket-58c9d`.

## Importante antes de publicar

Este commit é uma **base funcional de interface e fluxo inicial**, não um sistema pronto para receber pagamentos reais. Não introduza dados bancários secretos nem confie apenas em comprovativos.

### 1. Configuração Firebase Web

No Firebase Console, abra **Configurações do projeto → Geral → Os seus apps → Configuração do SDK**. Copie a configuração Web e substitua em `index.html`:
- `REPLACE_WITH_FIREBASE_WEB_API_KEY`
- `REPLACE_WITH_MESSAGING_SENDER_ID`
- `REPLACE_WITH_FIREBASE_APP_ID`

O `projectId` e `databaseURL` já correspondem ao projeto mostrado nas capturas. Confirme o `storageBucket` no console e ajuste se necessário.

### 2. Authentication

A captura mostra e-mail/palavra-passe e Google activados. Em **Authentication → Settings → Authorized domains**, acrescente o domínio de alojamento que utilizar. Teste recuperação de palavra-passe e verificação de e-mail antes do lançamento.

### 3. Realtime Database — regras

As regras mostradas nas capturas negam toda a leitura e escrita. Isso protege os dados, mas impede a app de funcionar até configurar regras adequadas. **Não substitua por `.read: true` e `.write: true` em produção.**

A implementação de demonstração ainda precisa de regras de segurança rigorosas e de funções de servidor para:
- verificar o papel administrativo a partir de uma fonte que o próprio utilizador não possa editar;
- validar vendedor, propriedade de produtos, preço, stock, encomendas e transições de estado;
- impedir que um cliente aprove o próprio pagamento ou altere o seu papel;
- guardar comprovativos e ficheiros digitais no Firebase Storage com regras privadas;
- calcular o total no servidor e validar notificações assinadas do gateway de pagamento.

Não atribua `role: "admin"` a partir de um formulário público. Configure o primeiro administrador manualmente com acesso controlado e proteja a função através de Firebase custom claims / Cloud Functions. O painel é uma interface; as regras do banco são a verdadeira barreira de segurança.

### 4. Pagamentos e ficheiros

O formulário de pagamento desta versão permite anexar um comprovativo para revisão, mas guarda temporariamente o conteúdo como texto na base de dados e limita o ficheiro a 5 MB. Isso é apenas para protótipo. Antes de usar com clientes, mova os comprovativos para **Firebase Storage**, defina permissões privadas, retenção e limites, e faça a aprovação através de funções seguras. A opção Multicaixa Express é apenas uma seleção de método neste protótipo — **não processa pagamentos reais**. Contrate e integre um gateway compatível com o serviço de pagamentos online/EMIS ou banco parceiro, validando as notificações no servidor.

Links de entrega digital não devem ser URLs públicas permanentes. Sirva os ficheiros só a compradores com pagamento confirmado, através de links temporários ou endpoint protegido. Defina política de reembolso, devolução, privacidade e moderação.

### 5. Executar e alojar

1. Abra **Firebase Console → Hosting** e configure Firebase Hosting (ou use outro alojamento HTTPS).
2. Publique `index.html`, `manifest.json`, `sw.js` e `icon.svg`.
3. Para alojamento local, pode usar qualquer servidor estático; não abra o ficheiro HTML directamente como `file://`.
4. Faça primeiro testes com contas de demonstração e produtos fictícios.

## Próximas etapas recomendadas

1. Concluir regras RTDB e funções Cloud Functions/Admin SDK.
2. Migrar comprovativos e ficheiros para Firebase Storage.
3. Integrar o gateway real e validar pagamentos no servidor.
4. Implementar recuperação de senha, verificação de e-mail, stock, expedição e notificações.
5. Validar legal/fiscalmente a operação e as condições para vendedores em Angola.

## Tecnologias

HTML/CSS/JavaScript, Firebase Authentication, Firebase Realtime Database e Progressive Web App.
