# Alibaba's X-Engine

Alibaba has long deployed their own fork of MySQL, [AliSQL](https://github.com/alibaba/alisql) to serve the highest peak throughput e-commerce workload in the world.

Recently, they have replaced AliSQL internal InnoDB engine with X-Engine, an LSM-based storage engine which they optimized for the high burst workload observed during the 11-11 Annual Sales Campaign.

With this note, I want to explore the optimization Alibaba have applied to their SQL Database and weight the tradeoffs.

## X-Engine Optimization

WIP

## Articles

1. [Facebook: Replaces InnoDB with RocksDB](https://github.com/facebook/mysql-5.6/wiki/MyRocks-advantages-over-InnoDB)

2. [Alibaba: PolarFS](https://www.vldb.org/pvldb/vol11/p1849-cao.pdf)

3. [Alibaba: X-Engine](https://www.cs.utah.edu/~lifeifei/papers/sigmod-xengine.pdf)

4. [Alibaba: LSM Compaction on FPGA](https://www.usenix.org/system/files/fast20-zhang_teng.pdf)

5. [LUDA: LSM Compaction on GPU](https://arxiv.org/pdf/2004.03054v1.pdf)
