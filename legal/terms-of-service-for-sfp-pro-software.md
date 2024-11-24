# Terms of Service for 'sfp-pro' Software

### 1. Ownership and License Grant

1.1. Flxbl Ltd (hereinafter referred to as "Licensor"), with a business address at 81-83, Campbell Street, Surry Hills, NSW, owns the copyright and all intellectual property rights to the 'sfp-pro' software (hereinafter referred to as "the Software"). The Software includes the sfp-pro cli as a nodejs executable, and a container image with sfp-pro pre-installed.

1.2. The Licensor grants the Licensee a non-exclusive, non-transferable license to use the Software with one designated Salesforce DevHub org (hereinafter referred to as "DevHub") and one designated Salesforce Production Org (hereinafter referred to as "Production").

1.3. The Licensor shall provide the Licensee with access to the source code of the Software. The Licensee is allowed to retain the source code for the duration of the license period and may continue to use the version of the source code in their possession after the expiration of the license, subject to the terms and conditions of this agreement.

1.4. The Licensor retains all intellectual property rights in the Software and any updates or modifications made by the Licensor. The Licensee's modifications to the Software do not grant them any intellectual property rights in the original Software.

### 2. License Renewal

2.1. The license will automatically renew at the end of the 12-month period unless the Licensee provides written notice of non-renewal at least 30 days prior to the end of the current license term.

2.2. The Licensor will send a renewal invoice to the Licensee 30 days before the end of the current license term. The Licensee must pay the renewal invoice within 30 days of receipt to ensure uninterrupted access to updates and support.

2.3. If the Licensee fails to pay the renewal invoice within 30 days, there will be a grace period of 15 days. The Licensee can email info@flxbl.io to discuss any further grace period.

2.4. If the Licensee does not pay the renewal invoice, they may continue to use the version of the Software in their possession at the time of license expiration, but will not receive any further updates or support.

2.5. The renewal price will be determined at the discretion of the Licensor and may be subject to annual increases. The Licensor will review and adjust the license costs annually.

### 3. Permitted Use and Modifications

3.1. The Licensee will be provided with the source code for the Software through an invite to the Licensor's source code repository.

3.2. The Licensee is allowed to modify any part of the Software and deploy it within the designated DevHub and Production, subject to the restrictions outlined in this agreement.

3.3. The Licensee may use the Software for any number of users within their organization, as long as it is used only with the designated DevHub and Production.

### 4. Restrictions

4.1. The Licensee is not allowed to transfer the rights or share the source code of the Software, or implement it in any DevHub or Production other than those designated in the license.

4.2. A separate license must be purchased for each combination of DevHub and Production where the Software will be used.

4.3. The Licensee acknowledges that the source code of the Software is confidential and agrees to take all necessary measures to prevent its disclosure to any third parties.

4.4. For each license of sfp-pro, the Software shall only be connected to one designated DevHub and one designated Production. The Licensee is not allowed to modify the code to connect to multiple DevHubs or multiple Productions under a single license.

4.5. The Licensee is permitted to use sfp-pro locally on a developer's machine, but its operations are restricted to the designated DevHub and Production.

4.6. The Licensee is not permitted to use the Software to provide managed services or any form of service to third parties. If the Licensee intends to use sfp-pro for client work or to offer it as a service, they must obtain a specific System Integrator (SI) license from the Licensor.

4.7. The Licensee is prohibited from distributing the sfp-pro software or the container image with sfp-pro pre-installed to third parties.

### 5. Updates, Support, and Prerequisites

5.1. The Licensee will have access to all updates released for the Software and associated documentation during the 12-month license period.

5.2. Updates will be delivered to the Licensor's source code repository. The Licensee and any authorized users of the Licensee (restricted by their email domain) will be provided read-only access to this repository for the duration of the license period on a self-service basis. The Licensee may create issues in the repository to report bugs or request new features.

5.3. There will be no additional costs associated with receiving updates during the license period.

5.4. The Licensor offers a single support tier for sfp-pro: Enterprise. The support and features included in this tier are as follows:

* Unrestricted source code access
* Unlimited team members
* Dedicated Slack channel for faster responses
* Priority support: Initial response within 48 hours
* Guided walkthrough for upcoming features
* Customized training sessions for your team (excluding CI/CD specific implementation)
* Quarterly strategic review and optimization sessions (focused on sfp-pro usage, not CI/CD workflows)
* Does not include CI/CD specific implementation or integration services

5.5. Definition of "Response" in Support Context:

a) A "response" in the context of support refers to an initial acknowledgment of the reported issue or inquiry by the Licensor's support team. This acknowledgment may include:

* Confirmation of receipt of the support request
* Request for additional information or clarification
* Provision of known workarounds, if available
* Acknowledgment of a bug or issue
* Estimated timeframe for further investigation or resolution, if possible

b) A "response" does not necessarily mean:

* An immediate fix or resolution of the reported issue
* A guarantee that the issue will be resolved within the response time frame
* A commitment to implement requested features or changes

c) The Licensor will make reasonable efforts to provide updates on the status of reported issues, but the time required for full resolution may vary significantly depending on the complexity of the issue, its impact on the Software's core functionality, and other factors.

d) The Licensor reserves the right to prioritize issues based on their severity, impact on users, and overall importance to the Software's functionality and security.

e) For complex issues or feature requests, the Licensor may, at its discretion, add the item to the product roadmap for future consideration, without committing to a specific implementation timeline.

5.6. Support is strictly limited to addressing issues and bugs directly related to the Software. The Licensor is not obligated to provide support for:

* General inquiries regarding usage or best practices
* Issues arising from the Licensee's modifications to the Software
* Problems related to the Licensee's environment or third-party services
* Training or education beyond what is explicitly included in the Enterprise tier
* CI/CD specific implementation or integration with the Licensee's existing workflows

5.7. Support will be provided during reasonable business hours, which are defined as 9:00 AM to 5:00 PM Australian Eastern Standard Time (AEST), Monday through Friday.

5.8. The Licensor reserves the right to refuse support for any customizations or modifications made by the Licensee to the Software.

5.9. All documentation for the Software will be provided at https://docs.flxbl.io/sfp.

5.10. The Software may require the use of additional prerequisites, including but not limited to GitHub Enterprise Subscription, GitLab, Azure Pipelines, Supabase (Self Hosted/Cloud), Datadog, and NewRelic. These prerequisites:

* Are not included in the cost of the sfp-pro license
* Will not be supported by the Licensor, except for specific integration points within sfp-pro
* Must be procured and installed by the Licensee at their own expense
* May be required for certain features of sfp-pro to function properly
* May become requirements for existing features in future updates

5.11. The Licensor reserves the right to augment existing features to have new prerequisites. While the Licensor will make best efforts to ensure backward compatibility, support for older versions will be limited to the three most recent releases.

5.12. The Licensee is responsible for ensuring their environment meets all necessary prerequisites for their desired sfp-pro functionality. Failure to meet these prerequisites may result in limited functionality or performance issues, for which the Licensor cannot be held responsible.

5.13. The Licensee acknowledges that while sfp-pro is predominantly used in CI/CD workflows, the Licensor does not provide any CI/CD specific implementation or integration services as part of the license purchase. Any such services, if required, must be separately negotiated and contracted.

### 6. Confidentiality

6.1. Both parties agree to maintain the confidentiality of any sensitive information exchanged during the course of this agreement, including but not limited to access credentials and customer data. Each party shall treat such information with the same degree of care as it treats its own confidential information, but in no event less than reasonable care.

### 7. Limitation of Liability

7.1. To the maximum extent permitted by applicable law, in no event shall the Licensor be liable for any indirect, incidental, special, consequential, or punitive damages, or any loss of profits or revenue, whether incurred directly or indirectly, or any loss of data, use, goodwill, or other intangible losses, resulting from: (i) the Licensee's access to or use of or inability to access or use the Software; (ii) any conduct or content of any third party on the Software; (iii) any content obtained from the Software; and (iv) unauthorized access, use, or alteration of the Licensee's transmissions or content.

### 8. Indemnification

8.1. The Licensee agrees to indemnify, defend, and hold harmless the Licensor and its affiliates, officers, agents, employees, and licensors from and against any claims, liabilities, damages, judgments, awards, losses, costs, expenses, or fees (including reasonable attorneys' fees) arising out of or relating to the Licensee's violation of these Terms of Service or the Licensee's use of the Software, including, but not limited to, any use of the Software's content, services, and products other than as expressly authorized in these Terms of Service or any modifications made by the Licensee to the Software.

### 9. Force Majeure

9.1. The Licensor shall not be liable for any failure or delay in performance under this agreement that is caused by circumstances beyond its reasonable control, such as acts of God, natural disasters, government actions, or internet service provider failures.

### 10. Assignment

10.1. The Licensee is not allowed to assign or transfer their rights and obligations under this agreement to a third party without the prior written consent of the Licensor.

### 11. Entire Agreement

11.1. These Terms of Service constitute the entire agreement between the parties and supersede any prior agreements or understandings, whether written or oral, relating to the subject matter of these Terms of Service.

### 12. Severability

12.1. If any provision of these Terms of Service is found to be invalid, illegal, or unenforceable, the remaining provisions shall continue in full force and effect.

### 13. Dispute Resolution

13.1. Any dispute arising out of or in connection with these Terms of Service shall be referred to and finally resolved by arbitration under the rules of the Courts of Victoria, Australia in Melbourne. The arbitration shall be conducted in the English language before a single arbitrator appointed in accordance with the said rules.

### 14. Notices

14.1. All notices under this agreement shall be delivered by email to info@flxbl.io.

### 15. Representations & Warranties; Disclaimer

15.1. **Mutual Representations and Warranties**. Each party represents and warrants it has validly entered into this Agreement and has the legal power to do so.

15.2. **Licensee Representations and Warranties**. The Licensee represents and warrants that it: (i) is entitled to transfer, or enable the transfer of, all Licensee Data to the Licensor; (ii) has all rights necessary to grant the Licensor the licenses set forth in this Agreement; and (iii) will not transmit any Prohibited Content to the Licensor whether by means of the Software or as required for the Licensor's provision of Support hereunder. "Prohibited Content" refers to any content that: (a) infringes on any third party's intellectual property rights; (b) violates any applicable laws or regulations; (c) contains any malicious code or viruses; or (d) is otherwise harmful, threatening, or offensive.

15.3. **Disclaimer**. WITH THE EXCEPTION OF THE LIMITED WARRANTIES SET FORTH IN THIS SECTION 15, THE SOFTWARE IS PROVIDED "AS IS" TO THE FULLEST EXTENT PERMITTED BY LAW. THE LICENSOR AND ITS LICENSORS EXPRESSLY DISCLAIM ALL OTHER WARRANTIES, EXPRESS OR IMPLIED, INCLUDING WARRANTIES OF PERFORMANCE, MERCHANTABILITY, FITNESS FOR ANY PARTICULAR PURPOSES, AND NON-INFRINGEMENT. THE LICENSOR DOES NOT WARRANT THAT THE SOFTWARE: (I) IS ERROR-FREE; (II) WILL PERFORM UNINTERRUPTED; OR (III) WILL MEET THE LICENSEE'S REQUIREMENTS.

### 16. Termination

16.1. This license agreement may be terminated by either party upon written notice to the other party if the other party breaches any material term of this agreement and fails to cure such breach within thirty (30) days after receiving written notice thereof.

16.2. Upon termination of this agreement, the Licensee shall cease all use of the Software and destroy all copies of the Software in its possession.

### 17. Expiration

17.1. This license agreement shall expire at the end of the 12-month license period unless renewed in accordance with Section 2.

17.2. Upon expiration of this agreement, the Licensee may continue to use the version of the Software in their possession at the time of license expiration, but will not receive any further updates or support.

### 18. Governing Law

18.1. This agreement shall be governed by and construed in accordance with the laws of Victoria, Australia.

### 19. Acceptance of Terms

19.1. By purchasing a license or paying the license fee for the Software, the Licensee acknowledges that they have read, understood, and agree to be bound by these Terms of Service. If the Licensee does not agree to these Terms of Service, they should not proceed with the purchase or use of the Software.

19.2. The Licensor reserves the right to update and change these Terms of Service from time to time without prior notice. Any new features or tools added to the Software shall also be subject to these Terms of Service. The Licensee's continued use of the Software after any such changes shall constitute their consent to such changes.
