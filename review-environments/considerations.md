---
icon: ring-diamond
---

# Considerations

When working with review environments in flxbl projects, consider the following to optimize your workflow and resource usage:

1. **Lease Management**:
   * Use the `--leaseFor` parameter in `sfp reviewenv fetch` to set appropriate lease durations for your processes.
   * Balance between long enough leases to complete processes and short enough to allow efficient reuse.
2. **Wait Times**:
   * Utilize the `--wait` parameter in `sfp reviewenv fetch` to allow for environment availability in busy systems.
   * Set wait times based on your project's typical environment turnover time.
3. **Status Transitions**:
   * If a process completes before its lease expires, transition the environment to 'Available' status to allow earlier reuse within the same issue.
   * Keep environment statuses up-to-date to reflect their current use accurately.
4. **Automation**:
   * Integrate `sfp reviewenv` commands into your CI/CD pipelines for automatic environment management.
   * Include proper lease handling and status transitions in your automated workflows.
5. **Efficient Reuse**:
   * Understand the interplay between the 24-hour validity period and the `leaseFor` duration.
   * Design your workflows to maximize environment reuse efficiency within issues.
6. **Timely Unassignment**:
   * Unassign environments promptly when they're no longer needed to free up resources.
   * Consider whether to return to the pool or mark as expired based on the environment's state and your project's needs.
7. **Pool Management**:
   * Regularly review and manage your environment pools.
   * Ensure a healthy balance of available environments, considering both the number of environments and typical lease durations.
8. **Extension Judiciousness**:
   * Use `sfp reviewenv extend` only when necessary to avoid tying up resources unnecessarily.
   * Consider implementing approval processes for extensions in long-running projects.
9. **Monitoring and Logging**:
   * Implement monitoring for environment usage, lease times, and pool availability.
   * Maintain logs of environment assignments and transitions for troubleshooting and optimization.
10. **User Education**:
    * Ensure team members understand the review environment lifecycle and the impact of their actions on resource availability.
    * Provide guidelines for efficient use of review environments in your development processes.

By considering these points, you can streamline your development process, improve resource utilization, and ensure efficient use of review environments in your flxbl projects.
