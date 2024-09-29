# Projeto de Análise de Arquivos DBF e Parquet

Tal código foi desenvolvido para realizar o gerenciamento das etapas de processamento, transformação e carga dos dados com base em totalizações básicas. Isto se fez necessário, devido a estrutura já definida no projeto, que é a seguinte: 
1. Primeiro, a aquisição dos dados do cliente que estão em formato dbf é carregada em uma pasta da rede;
2. Com os arquivos em dbf é feita uma conversão de extensão para csv, onde os dados serão processados, validados e caso necessário sofrerão algum tratamento. Após esse processo de crítica o resultado é convertido para extensão em parquet e são carregados em uma outra pasta na rede;
3. Por fim, todas as saídas em parquet são carregas em um banco de dados, que nesse caso específico é o oracle sql developer.

**Durante a execução do código algumas especificidades foram encontradas e apresentaram as seguintes soluções:**
- Inexistencia de uma padronização total das pastas em que os arquivos em dbf estão inseridos.
   - Para isso foi necessário criar a função 'encontrar_caminho_dbf', que a partir de uma inspeção recursiva, foi listado todos os possíveis diretórios dos arquivos
- Nem sempre as totalizações dos arquivos em parquet são verdadeiras, visto que caso o processo de crítica tenha sido rodado mais de uma vez os dados são repetidos dentro do mesmo arquivo. Além disso, dentro da função 'arq_parquet_uni' precisamos realizar a leitura de diversos arquivos em parquet, pois o processo de critica é feito em lotes a partir do limite de x linhas.
  - Para saber quantas linhas estão de fato repetidas em parquet, usamos as informações presente da coluna 'repeticoes' das tabelas do banco, que deveriam ser exatamente os parquets sem registros repetidos.
 
  Para a execução do código são necessárias a instalação de algumas bibliotecas:
- `pandas`
- `numpy`
- `simpledbf` (Converte arquivo DBF para arquivo CSV)
- `cx_Oracle` (para conexão com o banco de dados Oracle)
- `sqlalchemy`
- `simpledbf`
- `oracledb`

  
