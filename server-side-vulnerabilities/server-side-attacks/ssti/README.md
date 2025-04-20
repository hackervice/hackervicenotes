# SSTI

**What is a Template Engine?**

* **Definition**: A template engine is software that combines pre-defined templates with dynamic data to generate responses in web applications.
* **Purpose**: It allows for shared components (like headers and footers) across web pages, reducing code duplication and improving maintainability.
* **Examples**: Popular template engines include Jinja and Twig.

**How Template Engines Work**

1. **Inputs Required**:
   * **Template**: A predefined structure (string or file) that contains placeholders for dynamic data.
   * **Values**: Key-value pairs that the template engine uses to fill in the placeholders.
2. **Rendering Process**:
   * The process of generating a final output string from the template and values is called rendering.
   * **Example**:
     * Template: `Hello {{ name }}!`
     * If `name` is set to "vautia", the output will be: `Hello vautia!`
3. **Complex Operations**:
   * Modern template engines support advanced features like loops and conditions.
   * **Example**:
     *   Template:

         ```
         {% raw %}
         {% for name in names %}
         Hello {{ name }}!
         {% endfor %}
         {% endraw %}
         ```
     *   If `names` is `["vautia", "21y4d", "Pedant"]`, the output will be:

         ```
         Hello vautia!
         Hello 21y4d!
         Hello Pedant!
         ```

**Introduction to Server-Side Template Injection (SSTI)**

* **Definition**: SSTI occurs when an attacker can inject templating code into a template that the server later renders. This can lead to the execution of malicious code on the server.

**How SSTI Vulnerabilities Occur**

1. **Dynamic Values**:
   * Template engines typically handle user input securely when it is provided as values to the rendering function.
   * However, SSTI can happen if user input is inserted directly into the template string before rendering.
2. **Common Scenarios**:
   * If user input is included in the template string, it can lead to code execution during rendering.
   * Repeated rendering of the same template with user-modified output can also introduce SSTI risks.
   * Allowing users to modify or submit templates directly is a clear vulnerability.

By understanding template engines and the risks associated with SSTI, you can better secure your web applications against these vulnerabilities. Join us in our community to learn more about these concepts and enhance your cybersecurity skills!
