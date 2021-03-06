FROM 15918793994/elasticsearch:6.2.4

# 变量定义
ARG node
ARG hosts
ARG minimum_master_nodes
ARG elastic_password

# 环境变量
ENV node.name $node
ENV ELASTIC_PASSWORD $elastic_password
ENV CERTS_DIR /usr/share/elasticsearch/config/certificates/certs
ENV ES_JAVA_OPTS "-Xms512m -Xmx512m"
ENV xpack.license.self_generated.type trial
ENV xpack.security.enabled true
ENV xpack.security.http.ssl.enabled true
ENV xpack.security.transport.ssl.enabled true
ENV xpack.security.transport.ssl.verification_mode certificate
ENV xpack.ssl.certificate_authorities $CERTS_DIR/ca/ca.crt
ENV xpack.ssl.certificate $CERTS_DIR/$node/$node.crt
ENV xpack.ssl.key $CERTS_DIR/$node/$node.key
ENV discovery.zen.ping.unicast.hosts $hosts
ENV discovery.zen.minimum_master_nodes $minimum_master_nodes

# 传入重写后的x-pack验证类
ADD ./x-pack/* /tmp/x-pack

RUN yum install -y java-1.8.0-openjdk-devel.x86_64

# 破解x-pack
RUN mkdir /usr/share/elasticsearch/plugins/x-pack/x-pack-core/x-pack-core-6.2.4 \
	&& mv /usr/share/elasticsearch/plugins/x-pack/x-pack-core/x-pack-core-6.2.4.jar /usr/share/elasticsearch/plugins/x-pack/x-pack-core/x-pack-core-6.2.4  \
	&& cd /usr/share/elasticsearch/plugins/x-pack/x-pack-core/x-pack-core-6.2.4 \
	&& jar xvf x-pack-core-6.2.4.jar \
	&& rm -rf x-pack-core-6.2.4.jar \
	&& rm -rf org/elasticsearch/license/LicenseVerifier.class org/elasticsearch/xpack/core/XPackBuild.class \
	&& mv /tmp/x-pack/LicenseVerifier.class org/elasticsearch/license/ \
	&& mv /tmp/x-pack/XPackBuild.class org/elasticsearch/xpack/core/ \
	&& jar cvf x-pack-core-6.2.4.jar ./ \
	&& rm -rf ../x-pack-core-6.2.4.jar \
	&& mv ./x-pack-core-6.2.4.jar ../ \
	&& cd ../ \
	&& rm -rf x-pack-core-6.2.4 \
	&& chown elasticsearch ./x-pack-core-6.2.4.jar

# 安装elasticsearch-sql插件
ADD ./elasticsearch-sql.sh /tmp/elasticsearch-sql.sh
RUN bash /tmp/elasticsearch-sql.sh