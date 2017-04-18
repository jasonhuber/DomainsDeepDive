## Domains Deep Dive

#2017 04 18

This the repository for the example files for the Deep Dive.

#install

It really isn't an install, but more of a configuration change. 

The example starts with a simple IN clause in the secrity file.
You could start by creating a new domain with the file:
DomainDeepDive_Schema.xml

then apply the first security file:
DomainDeepDive_security.xml

That requires a user with one attribute of:
warehouseids with a value of something like 1,2,3,4,5

Next we realize we want to use this functiont which is hard-coded into the domain security file in more than one place. We can do that by creating a Groovy function for it.

We do this in the 
applicationContext-SemanticLayer.xml 

This file exists in the WEB-INF folder of the webapp jasperserver-pro

We add this function (which is really similar to another one in the file ;) ):

&lt;entry key="parseCSVtoIN">
                    <value>
                        def testAttrName = args[1].value
                        def testField = sqlArgs[0]
                        def category = (args.size() > 2) ? args[2].value : null;
                        def attrVal = attributesService.getAttribute(testAttrName, category, false)?.attrValue
                        if (attrVal != null) {
                            return testField + " in (" + attrVal.split(",").collect{"'" + it.trim() + "'"}.join(",") + ")"
                        } else {
                            return "1 = 1"
                        }
                    </value>
                </entry>
				
				under:
				  <bean id="defaultSQLGenerator" class="com.jaspersoft.commons.semantic.dsimpl.SQLGenerator" scope="prototype">
        <property name="functionTemplates">
            <map>
			
			at about line 400.
			
Now we can change the domain security file to call that function:
DomainDeepDive_security_version2.xml




