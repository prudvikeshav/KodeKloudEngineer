# **Problem Statement:**

The Nautilus Application development team has finished development of one of the applications and it is ready for deployment. It is a guestbook application that will be used to manage entries for guests/visitors. As per discussion with the DevOps team, they have finalized the infrastructure that will be deployed on Kubernetes cluster. Below you can find more details about it.


BACK-END TIER

- Create a deployment named redis-master for Redis master.

     - Replicas count should be 1.

     - Container name should be master-redis-xfusion and it should use image redis.

     - Request resources as CPU should be 100m and Memory should be 100Mi.

     - Container port should be redis default port i.e 6379.

- Create a service named redis-master for Redis master. Port and targetPort should be Redis default port i.e 6379.

- Create another deployment named redis-slave for Redis slave.

     - Replicas count should be 2.

     - Container name should be slave-redis-xfusion and it should use gcr.io/google_samples/gb-redisslave:v3 image.

     - Requests resources as CPU should be 100m and Memory should be 100Mi.

     - Define an environment variable named GET_HOSTS_FROM and its value should be dns.

     - Container port should be Redis default port i.e 6379.

- Create another service named redis-slave. It should use Redis default port i.e 6379.

FRONT END TIER

- Create a deployment named frontend.

     - Replicas count should be 3.

     - Container name should be php-redis-xfusion and it should use gcr.io/google-samples/gb-frontend@sha256:cbc8ef4b0a2d0b95965e0e7dc8938c270ea98e34ec9d60ea64b2d5f2df2dfbbf image.

     - Request resources as CPU should be 100m and Memory should be 100Mi.

     - Define an environment variable named as GET_HOSTS_FROM and its value should be dns.

     - Container port should be 80.

- Create a service named frontend. Its type should be NodePort, port should be 80 and its nodePort should be 30009.

Finally, you can check the guestbook app by clicking on App button.
