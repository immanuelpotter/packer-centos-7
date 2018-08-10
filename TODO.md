## TODO
# It appears AWS doesn't currently support CentOS 7.5 - only 7.0/7.1 is specified on AWS website.
  This could be why I'm met with "Unsupported OS type / missing OS files" when doing the VM import, after the built image is uploaded to S3.
The point of using the amazon-import (A last resort) post-processor was to get the 7.5 minimal image into Amazon. The newest Source AMI I can find is CentOS 7.1.

I should create an amazon-ebs builder using this Source AMI and provision on top of that;
however, I wanted CentOS 7.5.
