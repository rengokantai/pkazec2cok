#### pkazec2cok
#####6 AWS data service
######elasticache
create subnet group
```
aws elasticache create-cache-subnet-group --cache-subnet-group-name yd --cache-subnet-group-description "yd" --subnet-ids subnet-111 subnet-222 --region us-east-1
```
create cluster(As of 03/2016, redis is not available for free tier, I use memcached)
```
aws elasticache create-cache-cluster --cache-cluster-id ydcluster --engine memcached --cache-node-type cache.t2.micro --num-cache-nodes 1 --engine-version 1.4.24 --cache-subnet-group-name yd --region us-east-1
```
add secuirty group
```
aws ec2 authorize-security-group-ingress --group-id sg-33e29a54 --protocol tcp --port 11211 --cidr 0.0.0.0/0 --region us-east-1
```

verify
```
aws elasticache describe-cache-clusters --cache-cluster-id ydcluster --region us-east-1
```
Redis supports singlenode clusters and replication groups, cannot partition your data across multiple redis clusters.  
In redis, scaling is achieved by choosing a different node instance type.(If app is read-intensive,you can create multi read replicas to distribute the load
######RDS
```
aws rds create-db-instance --db-instance-identifier yddb --allocated-storage 10 --db-instance-class db.t2.micro --engine mysql --master-username root --master-user-password password --region us-east-1
```
verify:
```
aws rds describe-db-instances --db-instance-identifier yddb --region us-east-1
```
