Its working is explained as follows:

+   - The following is the startup script `angular_component.dart`:
+   ```
+   import 'package:angular/angular.dart';
+   import 'package:angular/application_factory.dart';
+   import 'package:angular_component/salary/salary_component.dart';
+   import 'package:angular_component/job_listing.dart';
+   
+   void main() {
+     applicationFactory()
+       .addModule(new AppModule())
+       .run();
+   }
+   
+   class AppModule extends Module {
+     AppModule() {
+       bind(JobListingController);
+       bind(SalaryComponent);
+     }
+   }
+   ```

+   - In the web page, we see a new HTML element `<x-salary>` for our component, as follows:
+   ```
+   <x-salary max-sal="10" salary="job.rate_salary"></x-salary>
+   ```
+   This is also found in the job details section, which is now surrounded by the following code:
+   ```
+   <div ng-if="ctrl.selectedJob != null">
+   ...
+   <x-salary max-sal="10" salary="ctrl.selectedJob.rate_salary"></x-salary>
+   ...
+   </div>
+   ```
+   The component is defined in the folder `lib\salary`, with an HTML file `angular_component.html` that defines its structure, a Dart file that describes its behavior, and a CSS file to style it.

+   - The following is the Dart code:
+   ```
+   import 'package:angular/angular.dart';
+   
+   @Component(
+     selector: 'x-salary',
+   ```