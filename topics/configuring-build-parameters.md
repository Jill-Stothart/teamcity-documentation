[//]: # (title: Configuring Build Parameters)
[//]: # (auxiliary-id: Configuring Build Parameters)
[//]: # (Internal note. Do not delete. "Configuring Build Parametersd72e3.txt")

_Build parameters_ are name-value pairs, defined by a user or provided by TeamCity, which can be used in a build. They help flexibly share settings and pass them to build steps.

This article explains how to configure build parameters. See how to [use them inside build settings and build scripts](using-build-parameters.md).

## Types of Build Parameters

There are three types of build parameters in TeamCity:
* [Environment variables](#Environment+Variables)
* [System properties](#System+Properties)
* [Configuration parameters](#Configuration+Parameters)

<anchor name="ConfiguringBuildParameters-BuildParameters"/>

### Environment Variables

Environment variables can be passed into a spawned build process as into an environment.

They are defined by the `env.` prefix.

### System Properties

System properties can be passed into build scripts of [certain runners](using-build-parameters.md#Using+Build+Parameters+in+Build+Scripts) as variables specific to a build tool.

They are defined by the `system.` prefix.

<anchor name="ConfiguringBuildParameters-ConfigurationParameters"/>

### Configuration Parameters

Configuration parameters are not passed into a build process and are only meant to share settings within a build configuration. They are the primary means for customizing a build configuration which is based on a [template](build-configuration-template.md) or uses a [meta-runner](working-with-meta-runner.md).

They come with no prefix.

<anchor name="parameter-reference"/>

## Parameter Name Restrictions

The names of configuration parameters must contain only the `[a-zA-Z0-9._-*]` characters and start with an ASCII letter.

## Predefined Build Parameters

TeamCity provides a set of _predefined parameters_ that can be used within a build configuration settings or directly inside build steps. For example, you can access the current build's number by calling the respective parameter generated by TeamCity. See the [list of predefined parameters](predefined-build-parameters.md) for details.

## Custom Build Parameters

In __Build Configuration Settings | Parameters__, project administrators can define build parameters for the current build configuration. As soon as a new build starts in this configuration, TeamCity passes these parameters to its build scripts and environment.

>It is possible to redefine the parameters' values in a single build run by launching a [custom build](running-custom-build.md).

Build parameters defined in a build configuration are used only within this configuration. See how to define them on a [project or agent level](levels-and-priority-of-build-parameters.md).

Any user-defined build parameter (<emphasis tooltip="system-property">system property</emphasis> or <emphasis tooltip="environment-variable">environment variable</emphasis>) can reference other parameters as follows: `%\system.parameter_name%` or `%\env.parameter_name%`. For example, `system.tomcat.libs=%\env.CATALINA_HOME%/lib/*.jar`.  
Read more about parameter references [below](#Parameter+References).

You can also configure a [parameter's type](typed-parameters.md), so the parameter is displayed as a UI field in the _Run Custom Build_ dialog. This way, users will be able to use the UI dialog to quickly change the parameter's value in the next build run.

## Parameter References

Most text-field settings in TeamCity support referencing a build parameter as a variable. If you enter a string in the `%\parameter.name%` format, TeamCity will substitute it with the actual value during the build.

If a build references a parameter which is not defined, TeamCity will consider it an [implicit agent requirement](agent-requirements.md#Implicit+Requirements): the build will only run on the agents where this parameter is defined.  
The references to parameters which names do not satisfy the [above restrictions](#Parameter+Name+Restrictions) do not create an [implicit requirement](agent-requirements.md#Implicit+Requirements) and are ignored.

See [where you can use parameter references](using-build-parameters.md#Where+References+Can+Be+Used).

<seealso>
        <category ref="admin-guide">
        <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="levels-and-priority-of-build-parameters.md">Project- and Agent-Level Build Parameters</a>
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
            <a href="typed-parameters.md">Typed Parameters</a>
            <a href="configuring-agent-requirements.md">Configuring Agent Requirements</a>
        </category>
</seealso>