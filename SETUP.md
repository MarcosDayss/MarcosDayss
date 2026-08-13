# Atualização automática do perfil

O repositório `MarcosDayss/MarcosDayss` é o perfil público da conta no GitHub. O arquivo `README.md` é exibido na página principal do perfil, e `README.en.md` contém a versão em inglês.

## Como a atualização funciona

O workflow `.github/workflows/profile.yml` executa:

- nos dias ímpares do mês, às `06:20 UTC` — atualmente `03:20` em São Paulo;
- manualmente por `Actions > Update GitHub Profile > Run workflow`;
- sempre que o próprio arquivo do workflow é alterado.

O GitHub pode atrasar alguns minutos as execuções agendadas. Em repositórios públicos sem atividade por 60 dias, o GitHub também pode desabilitar workflows agendados; nesse caso, reative o workflow na aba Actions.

A automação regenera e versiona estes arquivos:

- `profile/contribution-snake.svg`;
- `profile/contribution-snake-dark.svg`;
- `profile/streak-pt.svg`;
- `profile/streak-en.svg`;
- `profile/stats-pt.svg`;
- `profile/stats-en.svg`;
- `profile/languages-pt.svg`;
- `profile/languages-en.svg`.

Os READMEs apontam diretamente para esses arquivos locais. Assim, o perfil continua renderizando mesmo quando os serviços usados para gerar os SVGs estão temporariamente indisponíveis.

## Secrets necessários

Acesse `Settings > Secrets and variables > Actions` no repositório.

### `PROFILE_STATS_TOKEN` — obrigatório

Esse token permite gerar os cards de estatísticas e linguagens com os repositórios pessoais públicos e privados autorizados.

A opção recomendada é um fine-grained personal access token pertencente à conta `MarcosDayss`:

1. selecione a própria conta como resource owner;
2. autorize apenas os repositórios pessoais que devem entrar nos agregados;
3. conceda `Contents: Read-only` em Repository permissions;
4. defina uma expiração e renove o secret antes dela;
5. salve o token como `PROFILE_STATS_TOKEN`.

Para incluir todos os repositórios privados pessoais, autorize todos eles. Repositórios de organizações não entram no cálculo.

### `STREAK_STATS_TOKEN` — opcional

O streak usa o gráfico de contribuições do perfil. Sem esse secret, o workflow tenta usar o token automático do GitHub Actions.

Se as contribuições privadas não forem contabilizadas corretamente, crie um personal access token clássico sem selecionar escopos e salve-o como `STREAK_STATS_TOKEN`. Esse token é usado somente pelo gerador do streak.

No perfil do GitHub, mantenha habilitada a opção de mostrar contribuições privadas para que elas apareçam de forma anônima no gráfico e no streak.

## Quais dados ficam públicos

Os arquivos em `profile/` são públicos e ficam no histórico Git. Os cards de estatísticas e linguagens agregam os repositórios pessoais autorizados, incluindo privados, e podem revelar:

- quantidade de repositórios públicos e privados;
- quantidade de commits de autoria da conta nas branches padrão;
- quantidade de repositórios pessoais com commits da conta;
- total de estrelas;
- proporção de linguagens por volume de código.

Eles não publicam nomes de repositórios privados, código ou mensagens de commit. O workflow também mascara nomes privados nos logs. Mesmo assim, só autorize repositórios cujos dados agregados você aceita tornar públicos.

O snake e o streak usam o gráfico de contribuições do GitHub. Contribuições privadas podem aparecer sem revelar o repositório de origem.

## Permissão para publicar as atualizações

O workflow declara `contents: write` para fazer commit dos SVGs. Se o passo `git push` falhar por permissão, confira:

`Settings > Actions > General > Workflow permissions > Read and write permissions`

Proteções da branch `main` também precisam permitir os commits do GitHub Actions.

Os commits automáticos usam a identidade `MarcosDayss <dev.salles7@gmail.com>`, mantendo o histórico de atualizações associado à sua conta.

## Atualização manual do conteúdo

Depois de alterar os READMEs, artes ou workflow:

```sh
git add .
git commit -m "Atualize o perfil do GitHub"
git push
```

Alterações em `.github/workflows/profile.yml` disparam uma execução automaticamente. Para qualquer outra alteração, execute manualmente em `Actions > Update GitHub Profile > Run workflow` se quiser regenerar os cards imediatamente.
