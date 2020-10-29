vault关键特性：
安全的机密信息存储：可以将任意密钥/值秘密存储在Vault中。vault先将这些机密加密，然后再将其写入持久存储中，因此获得对原始存储的访问不会泄露机密信息。保管箱可以写入磁盘，Consul等。

动态秘钥：vault可以按需为某些系统（例如AWS或SQL数据库）生成秘钥。例如，当应用程序需要访问S3存储桶时，它会向Vault要求提供凭据，并且Vault将根据需要生成具有有效权限的AWS密钥对。创建这些动态密钥后，vault还将在租约到期后自动将其撤消。

数据加密：保险柜可以加密和解密数据而无需存储。这使安全团队可以定义加密参数，并使开发人员可以将加密的数据存储在SQL等位置，而无需设计自己的加密方法。

租赁和续约：vault中的所有机密信息都具有与其相关的租赁。租约结束时，vault将自动撤消该机密信息。客户可以通过内置的续订API续订租约。

撤销：保险柜内置了对机密信息撤销的支持。vault不仅可以撤消单个机密信息，而且可以撤消机密信息树，例如，特定用户读取的所有机密信息，或特定类型的所有机密信息密。撤消有助于密钥滚动以及在入侵情况下锁定系统。

vault基于role，data，operation的权限分级机制：
1. vault支持多种auth method。不同的auth method会有不同的meta 数据。比如ldap会有group特性，因此可以将rolemap成group。
2. vault支持将资源抽象成path，不同的资源可以有不同的path。这样可以将不同的数据map成不同的path。
3. vault对于每个path可以支持read/write[create/update,read,delete,list]等操作，可以在policy中支持不同的权限
4. vault支持autho method和policy的绑定。比如对于不同的role（group）应用不同的policy。
上述四步结合起来可以达到基于role，data，operation的权限分级管理

我们的软件和vault的差别：
vault基于国际通用加解密算法，我们可以实现基于国密的加解密流程。
