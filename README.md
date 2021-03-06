# Análise de Log
Este projeto se baseia na analise de dados de um website atraves do uso de Sql query em ambiente Python para a rapida obtenção dos resultados. As metas da analise dos dados foram bem sucedidas e os resultados foram formatados usando funções e loops. Os resultados dessa analise não modificam os dados originais

## Etapas iniciais

Se já tiver algum dos programas listados abaixo e quiser usá-los, apenas certifique-se de estarem atualizados e instalados corretamente. Para o bom funcionamento e a visualização correta do projeto será necessário o download dos arquivos do item 5 e 6. 
<ol>
  <li>Fazer o donwload e instalar o <a href="https://www.python.org/downloads/">Python 3</a> (o projeto está na versão 3.7.2)</li>
  <li>Fazer o donwload e instalar o <a href="https://www.virtualbox.org/">Virtual Box</a></li> 
  <li>Fazer o download e instalar o <a href="https://www.vagrantup.com/downloads.html">Vagrant</a></li>
  <li>Fazer o download e instalar o <a href="https://git-scm.com/">Git</a></li>
  <li>Fazer o download do database zipado <code>newsdata.sql</code></li>
  <li>Fazer o download da pasta <a href="https://git-scm.com/">VM</a> com as pre-configurações do <code>Vagrant</code></li>
  <li>Depois de instalar o vagrant vá ao seu terminal e coloque na mesma pasta o banco de dados <code>newsdata.sql</code> e a pasta<code>VM</code></li>
  <li>Dentro desta mesma pasta use o comando <code>vagrant up</code> e logo em seguida <code>vagrant ssh</code> para acessar a VM</li>
  <li>Use o comando <code>psql -d news -f newsdata.sql</code> para executar as declaracoes e logo em seguida <code>psql -d news</code> para ter acesso ao banco de dados</li>
  <li>Comece criando as <code>views</code> que serão fornecidas logo abaixo para se obter os resultados desejados</li>
</ol>

## Criando as views

<p1>Criando a view <code>authorId</code> para identificar o autor e o seu artigo :</p1>
<pre>
  <code>
  create view authorId as 
  select title, name from articles, authors 
  where authors.id = articles.author and 
  authors.name not like 'Anonymous Contributor';
  </code>
</pre>


<p2>Criando a view <code>articleSum</code> para somar as views dos artigos:</p2>
<pre>
  <code>
  create view articleSum as
  select title, count(log.id) as views
  from log, articles
  where log.path = CONCAT('/article/', articles.slug)
  group by articles.title order by views desc;
  </code>
</pre>

<p3>Segue a view da terceira query <code>status</code>:</p3>
<pre>
  <code>
  create view status as select time::date, 
  (count(case when status != '200 OK' then 1 else null end)
  ::float/count(status)::float)*100 as percent 
  from log
  group by time::date
  order by percent desc;
  </code>
</pre>  
