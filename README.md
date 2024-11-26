## **Desafio Dasa**

### Sobre
Este projeto implementa um pipeline desenvolvido para atender ao desafio do processo seletivo para a vaga de bioinformática na Dasa. O Snakemake foi utilizado para integrar diferentes programas e scripts personalizados, permitindo a anotação de variantes genéticas a partir de um arquivo VCF. As anotações incluem o gene, o ID dbSNP e as frequências dos alelos de referência e alternativo em relação à população global do projeto 1000 Genomes 30x. Os resultados foram disponibilizados por meio de uma API e de uma interface web interativa desenvolvida com Flask, permitindo a filtragem de variantes por frequência e profundidade (DP).

Os scripts desenvolvidos para a anotação estão disponíveis em `/projeto_dasa/annotation/scripts`.
Os resultados da anotação são gerados no diretório `/projeto_dasa/annotation/results`.
Os scripts desenvolvidos para a interface estão disponíveis em `/projeto_dasa/interface`.
### Instruções
#### 1. Clone o repositório:
``` bash
git clone https://github.com/gbrl-mendes/projeto_dasa.git
```
#### 2. Construa a imagem do projeto:
``` bash
docker build -t projeto_dasa projeto_dasa/docker_dasa/
```
#### 3. Inicie o Container:
``` bash
docker run -it -p 8181:80 projeto_dasa
```
#### 4. Execute o Snakefile:
``` bash
snakemake --cores <X> #escolha um numero de cores de acordo com sua maquina
```
O Snakefile executa todas as etapas de anotação do arquivo VCF fornecido para o desafio. Os resultados da anotação são disponibilizados no diretório `/projeto_dasa/annotation/results`.

A última etapa, iniciada com a regra `get_frequencies`, envolve a recuperação das frequências populacionais das variantes, e é realizada por meio de um bash script que consulta o banco de dados NCBI SNP para obter as frequências. Este processo pode levar algumas horas, dependendo dos recursos disponíveis. Portanto, caso não queira aguardar, é possível interromper o pipeline. De qualquer forma, a interface conseguirá acessar os resultados, pois a anotação já foi previamente executada, e os resultados estão disponíveis no repositório `/projeto_dasa/interface/data`. Caso deseje visualizar os próprios resultados usando a interface, copie os arquivos ` NIST_dbSNPid_func_annot.vcf` e `NIST_ids_frequencies.txt` do diretório `/projeto_dasa/annotation/results` para o diretório `/projeto_dasa/interface/data`.

5. **Inicie o servidor Flask**:
``` bash
	python app.py
```
6. **Acesse a interface:**
	
	1. Se você construiu a imagem Docker em uma máquina local, abra o navegador e digite:
		http://localhost:8181
	2. Se você construiu a imagem Docker em um servidor, abra o navegador na sua máquina local e digite:
		http://<endereço_do_servidor>:8181

### Contato 
Para mais informações, entre em contato comigo através do meu endereço [e-mail](mailto:gabrielmendesbrt@outllok.com) 😊
