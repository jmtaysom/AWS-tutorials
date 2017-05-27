### Read throughput
One provisioned through put is equal to one 4 KB request per second. For example if you need to read 100 1 KB reads per second you would need to provision 100 read capacity units because you have 100 requests. Each request is rounded up to the nearest 4 KB chunk. If you need to respond to 50 10 KB reads per second then you would need 150 read capacity units. 10 rounds up to 12 which is 3 chunks multiplied by 50 is 150.

### Write throughput
One provisioned thoughput is equal to one 1 KB request per second. So if you need to write 50 5 KB requests per second then you would need to provision 250 write capacity units.
