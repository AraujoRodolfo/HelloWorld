# PROCEDIMENTOS INTERNOS DE LANÇAMENTO

###  1. EXPORTAR AS NOTAS DO PORTAL
  > No portal Ead, exporte apenas as colunas referentes às atividades, excluindo qualquer outra como: total, média..etc
###  2. ABRIR O ARQUIVO DO EXCEL:
 - DADOS -> FILTRO -> ORDEM ALFABETICA NO NOME DOS ALUNOS
 - EXCLUIR AS COLUNAS QUE NÃO SEJAM AS NOTAS 
 - RENOMEAR AS COLUNAS COMO P1, P2, P3... (OBSERVANDO O TÍTULO DA TAREFA)
 - REOGANIZAR A ORDEM DAS COLUNAS SE NECESSÁRIO: P1, P2, P3, P4...

   > CONSIDERE A ORDEM DAS COLUNAS DE NOTAS CRIADAS NO PORTAL, DEVERÁ SER A MESMA
 - FORMATAR AS NOTAS SEM CASAS DECIMAIS, SÓ NÚMEROS INTEIROS
 - SALVAR O ARQUIVO COMO CSV
###  3. NO PORTAL:
 - CRIAR AS COLUNAS DE NOTAS NO PORTAL SEGUINDO A ORDEM DAS COLUNAS DO EXCEL COMO SP1A1, SP1A2...SP2A1..ETC
 - CONFIGURAR A FÓRMULA DAS NOTAS PARA: **ROUND (N1+N2+N3+N4+N5+N6+N7,0)**

    > AS NOTAS JÁ TEM PESO CONFIGURADO NO PORTAL EAD, TOTALIZANDO 100. NÃO FAREMOS PESO SOBRE PESO
###  4. ABRIR A PLANILHA PARA DIGITAÇÃO DE NOTAS
 - ABRA O CONSOLE (CTRL+SHIT+i)
 - INSPECIONAR O PRIMEIRO CAMPO ONDE SERÁ DIGITADO A PRIMEIRA NOTA DO PRIMEIRO ALUNO
 - OBSERVAR A PROPRIEDADE "NAME" DO INPUT E IDENTIFICAR O CÓDIGO, POR EXEMPLO:

   > ctl00$MainContent$Notas$gdvNotas$ctl02$txt**490693**P1

 - O CÓDIGO É O QUE VEM APÓS "TXT" E ANTES DE "P1", NO EXEMPLO O CÓDIGO É: **490693**

##### CÓDIGO 
```js
function vai(codigo){
  const fileInput = document.createElement('input');
  fileInput.type = 'file';
  fileInput.accept = '.csv';
  fileInput.onchange = event => {
  const file = event.target.files[0];

  if (file) {
    const reader = new FileReader();
      
    reader.onload = function(e) {
      const csvContent = e.target.result;
      const rows = csvContent.split('\n');
      const header = rows[0].split(';').map(col => col.trim()); 
      
      for (let i = 1; i < rows.length; i++) {
          const columns = rows[i].split(';').map(col => col.trim()); 
          const rowIndex = String(i + 1).padStart(2, '0'); 
          columns.forEach(function(columnValue, colIndex) {              
              if (columnValue === "-") {
                  columnValue = "0";
              }              
              const fieldNumber = codigo + colIndex; 
              const fieldSuffix = header[colIndex].replace('P', ''); 
              const inputName = `ctl00$MainContent$Notas$gdvNotas$ctl${rowIndex}$txt${fieldNumber}P${fieldSuffix}`;
              const input = document.querySelector(`input[name="${inputName}"]`);
              if (input) {
                  input.value = columnValue; 
              }
          });
      }
    };    
    reader.readAsText(file); 
  }
};
fileInput.click();

}
vai(490709); // < ---- atualizar conforme inspecionado 

```
 - SUBSTITUIR O CÓDIGO OBSERVADO NO CAMPO, NA ÚLTIMA LINHA DE CÓDIGO ACIMA, EXEMPLO:

   >  Vai(**490693**);


### 5. A MÁGICA DE LANÇAMENTO RECORD:
- COPIAR O SCRIOT ACIMA COM ALTERAÇÃO DO CÓDIGO JÁ FEITA E COLAR NO CONSOLE

  > SERÁ NECESSÁRIO PERMITIR ESSA AÇÃO DIGITANDO O QUE SERÁ SOLICITADO NO CONSOLE

- COPIAR E COLAR NOVAMENTE O CÓDIGO E PRESSIONAR ENTER
- NA JANELA QUE SERÁ ABERTA, ESCOLHA O ARQUIVO CSV PREPARADO ANTERIORMENTE
- TODAS AS NOTAS SERÃO PREENCHIDAS

### 6. ATENÇÃO
  > OBSERVE QUE O BOTÃO "CALCULAR MÉDIA" É EXIBIDO:
  - CLIQUE EM ALGUMA DAS NOTAS E PRESSIONE TAB
  - DEPOIS, É SÓ SALVAR AS NOTAS
    > **CASO A PLATAFORMA EXIBA ERRO, ESTES PODEM SER OS MOTIVOS:**
    > - NOTAS PREENCHIDAS COM VÍRGULA (NÃO SÃO NÚMEROS INTEIROS)
    > - NÃO FOI CLICADO EM ALGUMA NOVA E PRESSIONADO TAB

### 7. PRONTO, VAI CURINTHIA!! ♪♫
