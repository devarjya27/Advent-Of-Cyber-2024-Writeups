Today we learn to hack a game with the help of `Frida`.

Frida is a powerful instrumentation tool that allows us to analyze, modify, and interact with running applications.  Frida creates a thread in the target process; that thread will execute some bootstrap code that allows the interaction. This interaction, known as the agent, permits the injection of JavaScript code, controlling the application's behaviour in real-time. One of the most crucial functionalities of Frida is the Interceptor. This functionality lets us alter internal functions' input or output or observe their behaviour. 
