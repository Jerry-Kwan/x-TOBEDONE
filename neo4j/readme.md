# ��������������:neo4j&server

��Ŀ¼�����neo4jͨ�ŵķ������ļ�

[��������������:neo4j&server](#��������������neo4jserver)

- [��������������:neo4j&server](#��������������neo4jserver)
  - [neo4j](#neo4j)
    - [��װ jdk11](#��װ-jdk11)
    - [��װ neo4j](#��װ-neo4j)
    - [�����������](#�����������)

## neo4j

### ��װ jdk11

+ ����ж�ط�������ԭ�����ܴ��ڵ� openjdk

  `sudo apt-get remove openjdk*`

+ ��[��Ϊ����վ](https://repo.huaweicloud.com/java/jdk/11.0.1+13/)����ѹ����

  `jdk-11.0.1_linux-x64_bin.tar.gz `

+ �ҵ�һ�����ʵ�·���������� `/usr/local`���½��ļ��У������н�ѹ��

  `sudo tar zxvf jdk-11.0.1_linux-x64_bin.tar.gz`

+ �Ӹ�Ŀ¼���� etc/profile �ļ�

  ```java
  export JAVA_HOME=/usr/local/jdk11/jdk-11.0.1
  export JRE_HOME=${JAVA_HOME}/jre
  export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
  export PATH=${JAVA_HOME}/bin:$PATH
  ```

  ע��Ҫ�����������һ�У�·�������޸�Ϊ�Լ����õ�λ��

+ ʹ�������ļ���Ч

  `source /etc/profile`

+ �鿴�Ƿ�ɹ���װ��

  `java -version`

  ![image-20220405145021107](image\image-20220405145021107.png)

  ע�����ܻ���֣�ʹ������ `source /etc/profile` ��ʹ�� `java -version`������ȷ��ʾ����������ص���ǰ�������ն��ٴ򿪺��ٴ����� `java -version` ȴ��ʾû�� java ������Ѿ��ϸ��������������ã���ô���������������

### ��װ neo4j

+ ���أ������޸İ汾��

  ` curl -O http://dist.neo4j.org/neo4j-community-4.4.0-unix.tar.gz`

+ ��ѹ��

  `tar -axvf neo4j-community-4.4.0-unix.tar.gz`

+ �ҵ���ѹ������ļ����޸������ļ����������ļ����� /neo4j-community-4.4.0/conf �е� neo4j.conf

  `sudo vim neo4j.conf`

+ ���Բο�[�������](https://blog.csdn.net/u013946356/article/details/81736232)�鿴����ϸ�Ĳο�������ֻ�оټ�����Ϊ�ؼ�������

  ���޸� load csv ʱ·�����ҵ�������һ�У�����ǰ��Ӹ� #���ɴ�����·����ȡ�ļ�
  dbms.directories.import=import

  �ڿ���Զ��ͨ�� ip ���� neo4j ���ݿ⣬�ҵ���ɾ��������һ�п�ͷ�� #

  dbms.default_listen_address=0.0.0.0

  �������Զ�� url �� load csv
  dbms.security.allow_csv_import_from_file_urls=true

  ������ neo4j �ɶ���д
  dbms.read_only=false

  ��Ĭ�� bolt �˿��� 7687��http �˿��� 7474��https �ؿ��� 7473���޸����£�		

  <img src="image\image-20220405151833045.png" alt="image-20220405151833045" style="zoom:67%;" />	

+ ��������ͬ������./neo4j stopֹͣ����

  `cd neo4j-community-4.4.0`

  `cd bin`

  `./neo4j start`	

+ ������鿴
  http://0.0.0.0:7474/
  ��¼�û�������Ĭ�϶��� neo4j
  �����޸�һ�����룬~~�����޸�Ϊ 11����Ϊ��~~

<img src="image\image-20220405153110974.png" alt="image-20220405153110974" style="zoom: 67%;" />

+ ע�����ܻ���ְ��������������ã��ܹ�����������ʾ neo4j �Ѿ�����������������򿪶�Ӧ��ַȴ�޷����أ���ʱ�����Ƿ�����Ϊ������ķ���ǽ���£��رշ���ǽָ�

  `sudo ufw disable`



### �����������

+ `pip3 install websockets`

  `pip3 install neo4j`

  ע�����û�� python �� pip3�����Ȱ�װ�� python��Ȼ�������������װpip

   `sudo apt-get install python3-pip`
+ �ҵ�λ��./web&server/main_serverĿ¼�µ�serverWeb���ļ�
  
    �ҵ����е�������ʾ����
    ```python
    if __name__ == "__main__":
    #�˿������û��������������Ҫ�Ķ�
    #create_newnode(node)���ڴ�����㣨��������ǩ��������ǩ�ڵ㡢�����Ӧ�ıߵȹ��ܣ�
    #delete_node(node.name)����ɾȥ��Ϊnode.name�Ľ��
    
    #�������ݿ� 
    scheme = "neo4j"  # Connecting to Aura, use the "neo4j+s" URI scheme
    host_name = "localhost"
    port = 7474
    url = "bolt://47.119.121.73:7687".format(scheme=scheme, host_name=host_name, port=port)
    user = "neo4j"
    password = "disgrafs"
    
    Neo4jServer = pytoneo.App(url, user, password)
    print("Neo4j���������ӳɹ�...")
    
    #����webserver������
    start_server = websockets.serve(main_logic, '0.0.0.0', 9090)
    print("����������ʼ���ɹ����ȴ�����...")
    
    asyncio.get_event_loop().run_until_complete(start_server)
    asyncio.get_event_loop().run_forever()
    ```
    - ��url�޸�Ϊ�������Ĺ���ip `bolt`�ֶ�������ʱ���������Ϊ�����������޸�Ϊ`neo4j://0.0.0.0:7687` 7687ΪĬ�϶˿� ���ʹ�õĲ���Ĭ�϶˿��������޸�
    - `user`��`password`�޸�Ϊǰ�ĵ�¼`neo4j://0.0.0.0:7474`ʹ�õ��˺�������
    - Ŀǰprint���ܽ���ʾ������������ **������ɹ�����**
+ ��� DisGraFS: /web&serer/main_server �£����з������ˣ�

  `python3 serverWeb.py`

  <img src="image\image-20220405155150240.png" alt="image-20220405155150240"  />

  ���ˣ�**�����������ɹ�**

  web&server/main_server ����ļ���Ҳ�����������ʹ��

> main_server ������ŵ�Ϊ������������������ļ���pytoneo.py �� serverWeb.py��pytoneo.py Ϊ�����ͳ��������� neo4j ���ݿ���н�����serverWeb.py ������ pytoneo.py �ĺ�����ʵ�ִ�����㣬ɾ�����ȹ��ܡ�

