# Race Conditions
A race condition is a situation in computer programs where the timing of events influences the behaviour and outcome of the program. It typically happens when a variable gets accessed and modified by multiple threads. Due to a lack of proper lock mechanisms and synchronization between the different threads, an attacker might abuse the system and apply a discount multiple times or make money transactions beyond their balance.

# Methodology
1. Create 2 separate accounts.
2. First, we need to explore and study how the target web application receives HTTP requests and how it responds to them using Burp Suite Proxy. **check what is the response for valid and invalid input**
3. You will see the `POST` request which sends the credit from one number to the other.
4. send the captured `POST` request to the repeater. In repeater in the request tab section you can see the "+" beside the request number.
5. click on "+" -> Create tab group -> add random group name and slect the ✔️ on the request you want to test on -> click on "create"
6. Once you create the duplicate of the request, right click on the request number (here, 1) -> click on "duplicate tab" -> duplicate it 20 times -> click on "duplicate"
7. Next, click the drop down arrow beside "send" -> select "Send group (parallel)" -> send the selected request type by clicking again on "Send group (parallel)"
