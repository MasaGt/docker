### Kubernatesとは

コンテナのオーケストレーションツール  
オーケストレーションツールとは  
  ↓  
複数のサーバーに配置されたコンテナを一括して管理・運用するためのさまざまな機能が提供されており、コンテナの使用による運用負荷を大幅に軽減できます。  
(https://www.fujitsu.com/jp/products/software/resources/feature-stories/cloud/orchestration/#:~:text=%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%82%AA%E3%83%BC%E3%82%B1%E3%82%B9%E3%83%88%E3%83%AC%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%83%84%E3%83%BC%E3%83%AB%E3%81%AB,%E5%A4%A7%E5%B9%85%E3%81%AB%E8%BB%BD%E6%B8%9B%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)


具体的な例:  
- コンテナが停止していないかの監視
- 負荷がかかっているコンテナを検出し、負荷を分散したり

----

#### 今までのDockerの勉強と違う所

複数の物理マシンに複数のコンテナがあり、それを統括、管理するための物がKubernates