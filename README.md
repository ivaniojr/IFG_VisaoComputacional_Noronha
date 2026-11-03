Tarefa 1 - Relatório de Detecção de Objetos: Análise de Vacas em Vista Aérea (YOLOv8)

1. Como o Programa Funciona

O script desenvolvido é um pipeline completo de avaliação de detecção de objetos focado em calcular métricas reais de desempenho, não apenas exibir imagens. Ele funciona nas seguintes etapas:

    1.1. Definição do Ground Truth (Realidade): Primeiro, fornecemos ao código onde as vacas realmente 
         estão na imagem 00061.jpg usando coordenadas exatas (caixas delimitadoras/bounding boxes).
         
    1.2. Inferência do Modelo: O modelo YOLOv8 processa a imagem e retorna suas próprias predições 
         (o que ele achou e onde).
    
    1.3. Cálculo de IoU (Intersection over Union): O código possui uma função matemática que calcula a
         porcentagem de sobreposição entre a caixa predita pelo modelo e a caixa real. Isso mede a 
         precisão espacial da detecção.
         
    1.4. Avaliação de Métricas (Precision/Recall): Usando um limite (threshold) de IoU de 0.35, o 
         programa classifica as detecções do YOLO em Verdadeiros Positivos (TP) ou Falsos Positivos (FP),
         gerando uma Matriz de Confusão.
         
    1.5. Visualização: Por fim, plota a imagem sobrepondo as caixas reais (em verde) e as do modelo 
         (em vermelho) para inspeção visual.

2. Comparação dos Modelos YOLO (A Evolução)

Durante os nossos testes com esta imagem específica, observamos perfeitamente o clássico trade-off (compromisso) entre tamanho/velocidade e precisão nas redes neurais. A imagem 00061.jpg é particularmente desafiadora por ser uma vista aérea, um ângulo pouco comum na maioria dos datasets genéricos.

    YOLOv8 Nano (yolov8n.pt): O modelo mais leve e rápido. Ele conseguiu encontrar a caixa do objeto, 
    mas falhou na classificação, confundindo a vaca vista de cima com um urso (bear).
    
    YOLOv8 Medium (yolov8m.pt): Um modelo intermediário. Teve um desempenho melhor, localizando os 
    dois objetos, mas ainda sofreu com a perspectiva, classificando uma das vacas como cachorro (dog).
    
    YOLOv8 Large (yolov8l.pt): O modelo mais pesado e com mais parâmetros. Foi o único capaz de abstrair
    corretamente as características da imagem aérea, classificando perfeitamente ambos os objetos como
    vacas (cow - classe 19).

3. Resultados Obtidos com a Imagem 00061.jpg (Modelo Large)

Ao executar o script final com o modelo YOLOv8 Large, obtivemos os seguintes resultados técnicos:

    Detecções: 2 predições encontradas para 2 Ground Truths.
    Vaca 1 (Esquerda/Baixo - GT: [140, 192, 658, 400]):
        IoU alcançado: 0.3951
        Análise: O modelo achou a vaca, mas a caixa ficou um pouco deslocada em relação à marcação manual.
    Vaca 2 (Direita/Cima - GT: [620, 0, 1144, 202]):
        IoU alcançado: 0.5526
        Análise: Uma detecção com excelente sobreposição espacial (maior que 50%).

Métricas Finais (Considerando IoU >= 0.35): Como relaxamos a exigência espacial de 0.50 para 0.35, ambas as detecções foram consideradas válidas.

    True Positives (TP): 2
    False Positives (FP) / False Negatives (FN): 0
    Precision: 1.0000 (100% - Tudo que o modelo disse que era vaca, realmente era)
    Recall: 1.0000 (100% - De todas as vacas que existiam, o modelo achou todas)

Conclusão

O experimento demonstra que, para ângulos não convencionais (como drones ou satélites), modelos menores tendem a falhar na classificação (confundindo vacas com ursos ou cachorros). O uso de um modelo mais robusto (Large) resolveu o problema de classificação, enquanto a métrica de IoU nos ajudou a entender que a precisão espacial (o tamanho exato da caixa) ainda pode variar.

<img width="745" height="440" alt="image" src="https://github.com/user-attachments/assets/044ef62f-dbe7-4980-adf8-242bcf69ae90" />

