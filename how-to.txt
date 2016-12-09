Using the error_framework transformation it's possible to manage errors and logging in a manageable and dynamic way.

Define how logging is handled by completing 'metadata/error-codes.xlsx'. 

When adding the transformation for logging, add it to a selected step with a 'failed' hop.
Simply pass the error code with the 'PRM_ERROR_CODE' parameter.
The error_framework transformation will read the parameter and get the wanted message from the xml.
