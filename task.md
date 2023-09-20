## Task
# criar cron
00 01 * * * root rm -rf /opt/sei/temp/*
00 01 * * * root rm -rf /opt/sip/temp/*

# banco de auditoria
- estudar banco de dados de auditoria e fazer procedimentos

Task now
# Sincronização dados NFS produção com Homologação

H - NFS
200.129.17.113
prod NFS
10.11.4.32

## Servidores
# servidor RH
# InfraMail
# JODConverter

# Sessão do usuário
# Configuração da URL /
# Configurar contêiner Solr para persistir índices
# Configurar rotina de subir contêineres após desligamento
# Configurar acesso maquina pra logar com AD
# Estudo do manual para implementações de funcionalidades
# Estudo e atualização do modulo wssei V1.0.0 para V2.0.0(Augusto)


Items SEI 4.0 (Instalação)
01 - Baixar Arquivos ✅
02 - Servidores ✅
04 - Bases de Dados ✅
05 - Configuração SIP ✅
06 - Configuração SEI ✅
07 - Acesso aos Sistemas com LDAP/AD ✅
08 - Carga e Sincronização de Usuários e Unidades
09 - Tabela de Parâmetros SIP(Área de negócios)
10 - Tabela de Parâmetros SEI(Área de negócios)
11 - Agendamentos
12 - Scripts
13 - Backup
14 - Auditoria
15 - Tabelas de Log
16 - HTTPS
17 - Formulário de Ouvidoria
18 - Login de Usuários Externos
19 - Pesquisa de Publicações
20 - Conferência de Documentos
21 - Configuração de Máquinas Cliente
22 - Solr
23 - Problemas Conhecidos e Soluções

Items SEI 4.0 (Atualização)
1. montar o novo ambiente de acordo o documento SEI-Instalacao-v4.0.pdf usando os fontes, arquivos de configuração e as bases de dados vazias disponibilizadas com a nova versão. Somente depois que o novo ambiente estiver funcionando seguir para o próximo passo;

2. fazer um backup das bases SIP e SEI e do repositório de arquivos do novo ambiente;

3. fazer um backup dos arquivos /opt/sip/config/ConfiguracaoSip.php e /opt/sei/config/Confi-
guracaoSEI.php do novo ambiente;

4. limpar os índices do Solr do novo ambiente executando os comandos abaixo no console do ser-
vidor Solr (assumindo porta 8983):

5. restaurar um backup das bases SIP e SEI e do repositório de arquivos de produção;

6. ajustar no novo ambiente o arquivo /opt/sip/ConfiguracaoSip.php a chave BancoSip para
apontar para o backup de produção;

7. ajustar no novo ambiente o arquivo /opt/sei/ConfiguracaoSEI.php as chaves BancoSEI e
1RepositorioArquivos para apontar para os backups de produção;

8. Alterar na tabela sistema da base SIP restaurada de produção as referências para o novo ambiente

9. rodar o script para atualização do sistema SIP em linha de comando:

9.1 Copiar o valor referente a CHAVE DE ACESSO SIP para o arquivo ConfiguracaoSip.php
sobrescrevendo o valor existente na chave SessaoSip/ChaveAcesso

10. rodar o script para atualização dos recursos do SEI no SIP em linha de comando
11. rodar o script para atualização do sistema SEI em linha de comando:
12. Executar a indexação do Solr por linha de comando

1. fazer uma cópia dos arquivos /opt/sei/config/ConfiguracaoSEI.php e /opt/sip/config/Confi-
guracaoSip.php do novo ambiente e após substituí-los pelos respectivos de produção;

2. Abrir o arquivo /opt/sip/config/ConfiguracaoSip.php (cópia de produção) para edição

2.1. alterar o apontamento do serviço na chave SipWsdl de servico=wsdl para servico=sip:

2.2. adicionar a chave ChaveAcesso no grupo SessaoSip com conteúdo vazio (ela será gerada
posteriormente pelo script de atualização):

2.3. remover de BancoSip as chaves UsuarioScript e BancoScript (se existirem):

2.4. remover a chave HostWebService:

3. Abrir o arquivo /opt/sei/config/ConfiguracaoSEI.php (cópia de produção) para edição
3.1. desativar temporariamente qualquer módulo instalado renomeando a chave Modulos para

3.2. alterar o apontamento do serviço na chave SipWsdl de servico=wsdl para servico=sip:

3.3. adicionar a chave ChaveAcesso no grupo SessaoSEI com conteúdo vazio (ela será gerada
posteriormente pelo script de atualização):

3.4. remover de BancoSEI as chaves UsuarioScript e BancoScript (se existirem):

3.5. remover de XSS a chave FiltrarConteudoConsulta (se existir):

3.6. remover na chave HostWebService as subchaves Edoc, Sip e Ouvidoria (se não existir
nenhuma outra subchave todo o conjunto pode ser removido):

3.7. o mecanismo de comunicação entre instalações SEI Federação fica desabilitado por pa-
drão, para ativá-lo é necessário incluir a chave raiz abaixo:

3.8. remover na chave SEI a subchave MaxMemoriaPdfGb (se existir):

2 Executar os passos da seção anterior (Roteiro) a partir do item 4 (limpar índices do Solr), considerando em todos os passos as referências para os dados de produção;

3 Reativar os módulos instalados restaurando o nome original para a chave SEI/Modulos 

A - autenticação pelo endereço do chamador continua disponível e funcionando. Entretanto
esta característica será removida em uma versão futura sendo recomendado o uso apenas de Chaves
de Acesso. Para migração dos serviços basta (a) alterar a sinalização de Autenticação no cadastro do
serviço, 

(b) gerar a chave de acesso e (c) passar o valor no parâmetro Identificacao das chamadas;
3Web Services SEI - Blocos
Foi adicionado o estado “Recebido” aos blocos sendo sinalizado quando o bloco não é da unidade
mas foi disponibilizado para ela. Afeta o serviço consultarBloco (estrutura RetornoConsultaBloco,
campo Estado) e a operação homônima da API de módulos (estrutura SaidaConsultarBlocoAPI atri-
buto Estado);
4Web Services SIP - Autenticação
No SIP também foi alterada a forma de autenticação dos serviços para Chave de Acesso (ver detalhes
na seção “Serviços SIP” do documento SEI-WebServices-v4.0.pdf). Os serviços listarPermissao, re-
plicarPermissao e replicarUsuario agora devem receber como primeiro parâmetro a chave de acesso
do sistema cliente. Afeta apenas instituições que integraram o SIP com outro sistema. Neste caso é
necessário (a) cadastrar o sistema no SIP, (b) liberar os serviços acessados no campo “Serviços Li-
berados para Acesso no SIP” no cadastro do sistema e (c) gerar uma chave de acesso por meio da
ação “Gerar Chave de Acesso” na tela de listagem de sistemas;
65Atualização do PHP e jQuery
O SEI utilizava a versão 5.6 do PHP e 1.12.4 da biblioteca jQuery e agora passou a utilizar a versão
7.3 do PHP e 3.4.1 para o jQuery. Se a instituição possuir módulos então pode ser necessária uma
revisão;
6Conversão XSS pra PDF
Agora se houver suspeita de XSS no conteúdo de um documento o sistema passará a exibi-lo no
formato PDF. Assim a chave XSS/ProtocolosExcecoes do arquivo ConfiguracaoSEI.php pode ficar
somente com os documentos identificados como falso positivo e que ainda estão em edição. Quando
o usuário colar algum conteúdo suspeito o sistema exibirá uma comparação das versões informando,
se possível, o trecho do documento com problema;
7Mecanismos de Busca
É recomendado que exista no diretório raiz dos servidores SEI/SIP um arquivo robots.txt sinali-
zando para os mecanismos de busca que eles não devem indexar o conteúdo, ex:
User-agent: *
Disallow: /
Isso sinaliza para os mecanismos de busca que eles não devem indexar. O arquivo deve ser
colocado na raiz do servidor, normalmente /var/www/html, mas este valor pode ser sobrescrito na
configuração do Virtual Host com DocumentRoot. Os fontes já incluem um robots.txt nos
diretórios “web” do SEI e SIP;
8Auditoria
Desde a versão 3.1.0 é possível mover os dados de auditoria para uma base separada tornando mais
rápida a pesquisa dos dados recentes e diminuindo consideravelmente o tamanho do backup (ver
seção Auditoria no documento de instalação);
9Formulário de Ouvidoria (somente para instituições que utilizam formulário personalizado)
Esta versão adiciona na API de Web Services e Módulos do SEI as operações para registro de Ouvi-
doria sendo necessário realizar alguns ajustes (lembrando que, em um primeiro momento, também é
possível utilizar o formulário padrão):
a) Cadastrar o sistema onde o formulário estará hospedado na lista de sistemas do SEI com Auten-
ticação por Chave de Acesso. Após, adicionar um serviço chamado, por exemplo, “Ouvidoria” e
cadastrar as operações “Registrar Ouvidoria”, “Listar Estados”, “Listar Cidades” e “Listar Tipos
de Processo da Ouvidoria”;
b) Gerar uma Chave de Acesso para o serviço “Ouvidoria” cadastrado;
c) Alterar as chamadas de Web Services no formulário personalizado de acordo com os serviços
padronizados registrarOuvidoria, listarCidades, listarEstados e listarTiposProcedimentoOuvido-
ria. Passar no campo IdentificacaoServico destes serviços a Chave de Acesso gerada.
Ver documento de Web Services para ajuda na execução dos passos anteriores.
