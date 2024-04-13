# Guia para Criar um Modelo de Previsão com Azure Machine Learning

Este guia fornece instruções passo a passo para criar e implantar um modelo de previsão usando Azure Machine Learning (AML).

## Criar um Machine Learning Workspace no Azure

1. Faça login no [Portal do Azure](https://portal.azure.com/) usando as credenciais da conta Microsoft.
2. Crie um recurso do Azure Machine Learning com as seguintes configurações:
   - Subscription: nome da sua subscrição criada.
   - Resource group: LAB-IA-900.
   - Name: laboratorioai900.
   - Region: East US.
   - Storage account: nova storage account padrão criada para o workspace.
   - Key vault: novo key vault padrão criado para o workspace.
   - Application insights: novo application insights padrão criado para o workspace.
   - Container registry: None.
3. Selecione Review + Create para criar o recurso.
4. Navegue para o [Azure Machine Learning Studio](https://ml.azure.com) e acesse o novo Workspace criado, no caso, laboratorioai900.

## Usar Aprendizado de Máquina Automatizado para Treinar um Modelo

1. No [Azure Machine Learning Studio](https://ml.azure.com/?azure-portal=true), navegue até a página Automated ML.
2. Crie um novo trabalho (job) do Automated ML com as seguintes configurações:

### Basic settings:
   - Job name: mslearn-bike-automl.
   - New experiment name: mslearn-bike-rental.
   - Description: Automated machine learning for bike rental prediction.
   - Tags: none.

### Task type & data:
   - Select task type: Regression.
   - Select dataset: Create a new dataset com as seguintes configurações:
     - Data type:
       - Name: bike-rentals.
       - Description: Historic bike rental data.
       - Type: Tabular.
     - Data source:
       - Select From web files.
       - Web URL: https://aka.ms/bike-rentals.
       - Skip data validation: do not select.
     - Settings:
       - File format: Delimited.
       - Delimiter: Comma.
       - Encoding: UTF-8.
       - Column headers: Only first file has headers.
       - Skip rows: None.
       - Dataset contains multi-line data: do not select.
     - Schema:
       - Include all columns other than Path.
       - Review the automatically detected types.
   - Select Criar.
3. Após criado o dataset, selecione o dataset bike-rentals para submeter o job do Automated ML.

## Análise do Melhor Modelo

Quando o job do Machine Learning é completado, é possível analisar o melhor modelo treinado pelo job.

1. Na aba Overview do Job do Machine Learning, é possível ver o resumo do melhor modelo.
2. Selecione o nome do Algoritmo.
3. Selecione a aba Métricas e selecione os gráficos "residuals" e "predicted_true" para analisar a performance do modelo.

## Implantar e Testar o Modelo

1. Na aba Modelo, selecione Deploy e use a opção Web Service para implantar o modelo com as seguintes configurações:
   - Name: predict-rentals.
   - Description: Predict cycle rentals.
   - Compute type: Azure Container Instance.
   - Enable authentication: Selected.
2. Aguarde o início da implantação do modelo.
3. Aguarde o sucesso da implantação do modelo.

## Testar o Modelo Implantado

1. Selecione Endpoints no menu esquerdo do Azure Machine Learning Studio e abra o endpoint em tempo real predict-rentals.
2. Selecione a aba Testar.
3. Insira o seguinte JSON no input dos dados:
```json
{
  "Inputs": { 
    "data": [
      {
        "day": 1,
        "mnth": 1,   
        "year": 2022,
        "season": 2,
        "holiday": 0,
        "weekday": 1,
        "workingday": 1,
        "weathersit": 2, 
        "temp": 0.3, 
        "atemp": 0.3,
        "hum": 0.3,
        "windspeed": 0.3 
      }
    ]    
  },   
  "GlobalParameters": 1.0
}
```

4. Clicar no botão Test

5. Verificar os resultados, deve ser parecido a isto:
```json
{
  "Results": [
    317.7548944115925
  ]
}
```

## Limpar
1. Na aba Endpoints, selecionar predict-rentals e selecionar Delete para confirmar a exclusão do ponto de extremidade
2. No portal Azure, selecionar o ResourceGroup utilizado para o laboratorio.
3. Clicar no botão Delete resource group, escrever o nome do resource group para confirmar a exclusão de grupo de recursos


## Recursos Adicionais

- Documentação do Azure Machine Learning: [Link](https://docs.microsoft.com/en-us/azure/machine-learning/)
- Tutoriais e Exemplos: [Link](https://github.com/Azure/MachineLearningNotebooks)

Siga este guia para criar, implantar e consumir seu modelo de previsão com sucesso no Azure Machine Learning.
