Project 6 Distributed Key Value System
There is only one class for this project, the Replica class, which represents a replica in a RAFT consensus protocol.

This class has an election timer. If a leader doesn't send a heartbeat for a certain period of time, there will be an election
Heartbeats are sent by the leader based on a timer

The program handles get and put requests by the client by redirecting them to the leader or storing them in a message queue.
Once the leader gets the client's request, it will send the relevant messages to other replicas and log the transaction

here are the main functions to know:
handle_put: handles a 'put' request from a client. Appends it to self's transaction log if self is leader. Queues it otherwise.
handle_get: handles a 'get' request from a client. Gets the value from self's store if self is leader. Redirectes it if not.
handle_update: handles an 'update' message (heartbeats) from the leader. Resets timer and more.
handle_vote_request: handles vote requests from other replicas during elections. Determines a reaction based on the term and log comparisons.
handle_vote_response: handles incoming vote responses during an election. Determines if self can become leader.
handle_append_entry: handles append entry requests from leader. updates the self's log to synchronize with leader's log and respondes appropriately.
handle_append_entry_response: handles responses to append entry RPCs. adjusts data and commits entries if consensus

Challenges faced:
Getting the log entries to perfectly synchronize was rather difficult. I spent a while to ensure that all log entries where
consistent enough that I was passing the partition and crash tests.

I also spent a while at the beginning, just getting the put and get requests and passing the first few simple tests. After I 
did the related homework(I think 8) and studied the RAFT protocols it became easier

I think I organized the code pretty neatly. I could have done some more abstraction, but I think that I might lead to some 
readability issues, for me.

Testing:
I tested using the config file tests. I also created a lot of prints when I was debugging stuff. 

