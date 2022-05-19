<p class="has-line-data" data-line-start="0" data-line-end="1">Parse YAML Config</p>
<ol>
<li class="has-line-data" data-line-start="1" data-line-end="3">Problem:<br>
A user has a YAML file that defines their service’s configuration for each  environment and region. The Ops team’s tooling uses the YAML file to provision  and update the service, but the tooling only accepts a single environment/region  combination at a time. Write a script using your preferred language to reduce a  user’s YAML file to a single environment/region combination according to input  parameters.</li>
<li class="has-line-data" data-line-start="3" data-line-end="12">Expectations:<br>
• Script reads YAML file from disk<br>
• Script accepts three inputs: a environment, a region and YAML file  location<br>
• Script uses configuration under the common key if the environment and/or  region input parameter does not exist as a key in the YAML file<br>
• Script sorts the keys of the reduced config alphabetically after<br>
service_name, team_name and cost_center<br>
• Script outputs reduced configuration as a JSON file<br>
• Script uploads JSON file to an S3 bucket and has exponential backoff  retry logic that will fail if the upload does not complete in 10m<br>
• Example</li>
<li class="has-line-data" data-line-start="12" data-line-end="13">Example Input YAML:</li>
</ol>
<pre><code class="has-line-data" data-line-start="14" data-line-end="55">---
metadata:
   service: some-service
   team: Some Team
   cost_center: 1564577
common:
   repo:
      url: git@git.corp.adobe.com:some-org/some-repo.git
      version: main
   helm:
      chart_name: some-chart
      release_name: sc
      helm_version: &quot;3&quot;
   notifications:
      deployments: &quot;#some-channel-deployments&quot;
      releases: &quot;#some-channel-releases&quot;
dev:
   va6:
      helm:
         values: /some/path/in/some-repo/dev/va6.yaml
   va7:
      helm:
         values: /some/path/in/some-repo/dev/va7.yaml
integration:
   va7:
      notifications:
         deployments: &quot;#some-channel-integration-deployments&quot;
      helm:
         values: /some/path/in/some-repo/dev/va7.yaml
production:
   va7:
      repo:
         version: v1.34.2
      helm:
         values: /some/path/in/some-repo/production/va7.yaml
   va6:
      helm:
         values: /some/path/in/some-repo/production/va6.yaml
      repo:
         version: v1.34.2
</code></pre>
<ol start="4">
<li class="has-line-data" data-line-start="55" data-line-end="58">Example: Here’s what a running script should look like sh-session:<br>
some_script dev va6 /some/path/to/config.yaml<br>
Output Json:</li>
</ol>
<pre><code class="has-line-data" data-line-start="59" data-line-end="83">{
   &quot;environment&quot;: &quot;dev&quot;,
   &quot;region&quot;: &quot;va6&quot;,
   &quot;configuration&quot;: {
      &quot;service_name&quot;: &quot;some-service&quot;,
      &quot;team_name&quot;: &quot;Some Team&quot;,
      &quot;cost_center&quot;: 1564577,
      &quot;helm&quot;: {
         &quot;chart_name&quot;: &quot;some-chart&quot;,
         &quot;helm_version&quot;: &quot;3&quot;,
         &quot;release_name&quot;: &quot;sc&quot;,
         &quot;values&quot;: &quot;/some/path/in/some-repo/dev/va6.yaml&quot;
      },
      &quot;notifications&quot;: {
         &quot;deployments&quot;: &quot;#some-channel-deployments&quot;,
         &quot;releases&quot;: &quot;#some-channel-releases&quot;
      },
      &quot;repo&quot;: {
         &quot;url&quot;: &quot;git@git.corp.adobe.com:some-org/some-repo.git&quot;,
         &quot;version&quot;: &quot;main&quot;
      }
   }
}
</code></pre>
<ol start="5">
<li class="has-line-data" data-line-start="83" data-line-end="94">General Rules:<br>
Please note that Adobe has considerable experience with publicly available  examples. We realize<br>
that many implementations exist on the Internet and are very familiar with them.  We will not<br>
hold copying existing source code against you since in real-life leveraging  existing code may, at<br>
times, be the best and quickest way to get a good result. Be aware though that  not all publicly<br>
available solutions are of good code quality and we will consider your choice as  part of the<br>
assessment. We will also not hold it against you if you choose to write the entire  example from<br>
scratch. If you do decide to copy code from third party resources, please  consider the following<br>
when submitting your code:<br>
• Declare which parts of the sample is your own code<br>
• Reference all copied code and where you took it from: do not remove  copyrights or comments. Any violation of copyrights or obfuscation tactics  will reflect negatively on your assessment.</li>
<li class="has-line-data" data-line-start="94" data-line-end="95">Please provide us with a link from where we can download your solutions. For  technical reasons we cannot accept email attachments.</li>
</ol>
