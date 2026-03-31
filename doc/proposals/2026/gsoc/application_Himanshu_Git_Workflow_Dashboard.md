### About

1. **Full Name:** Himanshu
2. **Contact info:** himanshu011raj@gmail.com
3. **Discord handle in our server:** @maiHydrogen
4. **Home page:** [Insert Link or N/A]
5. **Blog:** [Insert Link or N/A]
6. **GitHub profile link:** [maiHydrogen](https://github.com/maiHydrogen)
7. **Twitter, LinkedIn, other socials:** [LinkedIn](https://www.linkedin.com/in/himanshu-rajput-02980429a)
8. **Time zone:** GMT+5:30 (India Standard Time)
9. **Link to a resume:** [Insert Publicly Accessible PDF Link Here]

### University Info

1. **University name:** Indian Institute of Technology Guwahati - [Website](https://iitg.ac.in)
2. **Program you are enrolled in:** B.Tech in Civil Engineering
3. **Year:** 2023
4. **Expected graduation date:** June 2027

### Motivation & Past Experience

**1. Have you worked on or contributed to a FOSS project before? Can you attach repo links or relevant PRs?**
Yes. I recently came across the API Dash repository, and while my number of contributions is still growing, I have already started engaging with the codebase. I successfully got a PR merged that contributed to the documentation regarding platform-specific instructions for Android build failures due to AGP. 
* Merged PR: [Fix Android AGP Build Issue](https://github.com/foss42/apidash/pull/589)

I also submitted other PRs aimed at adding basic auth, API keys, and bearer tokens. While these were closed in favor of using the existing data model instead of creating a new one, the maintainers provided positive feedback that greatly helped my understanding of the architecture.
* [Basic Auth PR](https://github.com/foss42/apidash/pull/680)
* [API Key and Bearer Token PR](https://github.com/foss42/apidash/pull/689)

*Other Open Source Contributions:*
I actively participated in DigitalOcean’s Hacktoberfest ’25 and Hactoberfest '24.
In Hacktoberfest'25 I achieved Supercontributer achievement also.

**2. What is your one project/achievement that you are most proud of? Why?**
I am most proud of my project **[OneStop_ui](https://github.com/maiHydrogen/onestop_ui)**. It is A flutter package developed for the live android and ios app available on online stores which is being used by 8000+ peopole residing in the IIT Guwahati campus on daily basis.
this dependency is UI components library which is widley used throughout every UI/UX part of the Onestop App.

**3. What kind of problems or challenges motivate you the most to solve them?**
I am highly motivated by challenges that are entirely new to me and offer a steep learning curve. If there is nothing new to learn, tasks can feel monotonous. While diving into the unknown means I might face initial failures or take a bit more time to find the optimal solution, I am incredibly persistent and always ensure the task is completed. The harder the challenge, the more I learn, which is my ultimate driving force.

**4. Will you be working on GSoC full-time? In case not, what will you be studying or working on while working on the project?**
Yes, I have no major commitments. With my summer vacation in progress, I can dedicate a substantial amount of time—approximately 30-35 hours per week. I am highly confident in my ability to fulfill the project requirements within the stipulated deadlines. Alongside the project, I will be continuously learning more about the specific tools required for API Dash and dedicating a little time to studying machine learning concepts to assist with the AI agent integration.

**5. Do you mind regularly syncing up with the project mentors?**
Absolutely not. I am fully available to sync up with the project mentors for regular updates, feedback, and guidance.

**6. What interests you the most about API Dash?**
API Dash is a highly promising API client built using Flutter. The most appealing aspect to me is the technical depth of the project—using Dart not just for building a beautiful frontend, but for robust, core application logic, rather than relying on third-party tools for the backend. The elegant use of Riverpod and Melos in the architecture is also fascinating. API Dash is full of new concepts for me to learn and master, and the maintainers are highly approachable and responsive.

**7. Can you mention some areas where the project can be improved?**
Currently, managing complex, multi-step API workflows can be highly manual. Introducing a visual node-based editor would drastically reduce the cognitive load on users trying to chain requests. Furthermore, collaboration is a key area for growth; enabling standard Git workflows directly inside the client for sharing environments and collections will make API Dash a team-ready tool. Lastly, providing a unified telemetry dashboard for test coverage and health metrics will elevate API Dash from a simple client to a comprehensive API testing suite.

**8. Have you interacted with and helped the API Dash community?**
Yes, I am active on the API Dash Discord server (@maiHydrogen) where I follow architectural discussions and help test out ideas. I have also interacted directly with maintainers on GitHub through the PRs mentioned above, taking their feedback to improve my understanding of the codebase.

---

### Project Proposal Information

**1. Proposal Title**
Git Integration, Agentic UI Workflow Builder & Collection Dashboard Implementation

**2. Abstract**
While API Dash currently excels as a fast, open-source API client, scaling its usage for team environments requires robust collaboration and automated testing tools. This project tackles these challenges by transforming API Dash into a comprehensive, intelligent API development suite. I will implement three core pillars: (1) **Git Integration**, enabling seamless version control and team sharing for collections and environments directly within the app; (2) a **Visual Workflow Builder**, featuring an Agentic AI that generates node-based API workflows from natural language prompts, alongside a manual drag-and-drop UI for chaining complex requests; and (3) a **Collection Dashboard**, providing a unified, metric-driven view of request histories, test coverage, and automated webhook reporting.

**3. Detailed Description**

The project requires a mix of state management, UI/UX design, local file/process management, and AI integration. Here is the technical breakdown and initial approach for each feature using Flutter and Dart:

* **Pillar 1: Git Integration for Version Control**
    Instead of reinventing version control, the app will leverage local Git installations via Dart's `Process` class to manage the initialization, committing, pushing, and pulling of API collections. 
    
    *Proposed Snippet (Git Command Wrapper):*
    ```dart
    Future<String> executeGitCommand(List<String> args, String workingDir) async {
      final result = await Process.run('git', args, workingDirectory: workingDir);
      if (result.exitCode == 0) {
        return result.stdout.toString().trim();
      } else {
        throw Exception('Git Error: ${result.stderr}');
      }
    }
    // Usage: await executeGitCommand(['commit', '-m', 'Update collection'], projectDir);
    ```

* **Pillar 2: Visual Workflow Builder & Agentic AI**
    I will develop an interactive, infinite-canvas workflow builder. Users will be able to drag-and-drop "Nodes" to chain requests. The Agentic AI will map natural language prompts to these nodes. 
    
    *Proposed Snippet (Core Node Data Model):*
    ```dart
    enum NodeType { request, delay, condition, extract }

    class WorkflowNode {
      final String id;
      final String label;
      final NodeType type;
      final Map<String, dynamic> payload; // Stores API Request ID or script logic
      final List<String> nextNodeIds; // For visual chaining and execution flow

      WorkflowNode({
        required this.id, 
        required this.label, 
        required this.type, 
        this.payload = const {}, 
        this.nextNodeIds = const []
      });
    }
    ```

* **Pillar 3: Collection Dashboard & Webhooks**
    I will implement a local logging system to track request execution histories and build a responsive dashboard UI utilizing charting libraries. Furthermore, I will add Webhook capabilities to automate reporting.
    
    *Proposed Snippet (Webhook Execution):*
    ```dart
    Future<void> triggerRunReportWebhook(String url, CollectionReport report) async {
      try {
        final response = await http.post(
          Uri.parse(url),
          headers: {'Content-Type': 'application/json'},
          body: jsonEncode(report.toJson()),
        );
        if (response.statusCode >= 400) {
          debugPrint('Webhook delivery failed: ${response.statusCode}');
        }
      } catch (e) {
        debugPrint('Webhook Error: $e');
      }
    }
    ```

**4. Weekly Timeline (175 Hours Scope)**

* **Weeks 1-2: Community Bonding & Architecture Design**
    * Discuss and finalize UI/UX wireframes for the Workflow Canvas and Dashboard with mentors.
    * Finalize the approach for Git integration.
    * Define the JSON schema for saving and exporting Node Workflows.
* **Week 3: Git Core Integration**
    * Implement backend Dart logic to execute Git commands (init, clone, commit, push, pull, status).
    * Ensure proper error handling for merge conflicts and authentication failures.
* **Week 4: Git UI & State Management**
    * Build the Source Control UI panel.
    * Wire the UI to the core Git logic using Riverpod, allowing users to commit and sync their collections from the app.
* **Week 5: Workflow Builder - Canvas UI**
    * Develop the interactive drag-and-drop canvas.
    * Create UI components for different node types.
* **Week 6: Workflow Builder - Chaining & Execution Logic**
    * Implement the execution engine that runs nodes sequentially.
    * Build the variable extraction logic (passing a token from Request A to Request B).
* **Week 7: Agentic AI Integration**
    * Implement the prompt input UI and backend connection to the AI service.
    * Write the parsing logic that translates the AI's response into the native node-based schema and renders it on the canvas.
* **Week 8: Dashboard - Data Aggregation**
    * Set up local storage schemas to log execution histories, response times, and status codes.
* **Week 9: Dashboard - UI & Visualization**
    * Develop the Dashboard UI utilizing charting libraries to display health metrics, success rates, and test coverage.
* **Week 10: Webhooks & Automation**
    * Build the Webhook configuration UI.
    * Implement the logic to trigger POST requests with summary payloads upon collection run completion.
* **Week 11: End-to-End Testing & Refinement**
    * Conduct rigorous testing of the workflow graph to handle edge cases.
    * Test Git integration across different operating systems (Windows, macOS, Linux).
* **Week 12: Buffer, Documentation, and Final Submission**
    * Fix any lingering bugs identified during testing.
    * Write comprehensive documentation for users on how to use the new features.
    * Submit the final code and GSoC evaluations.
