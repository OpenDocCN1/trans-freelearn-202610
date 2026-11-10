```
<!-- classpaths-->
<property name="dfcbase_jar"
    value="${documentum_jar_location}\dfcbase.jar"/>
<property name="dfc_jar" value="${documentum_jar_location}\dfc.jar"/>

<path id="totalClassPath">
    <pathelement location="${dfcbase_jar}"/>
    <pathelement location="${dfc_jar}"/>
</path>

<target name="all_targets" depends="init, compile_code,
            copyFolders, createZip"/>

<target name="init">
    <echo message = "Starting init" />
    <mkdir dir="${build_dir}"/>
    <mkdir dir="${build_class}"/>
    <echo message = "Finishing init" />
</target>

<target name="compile_code" depends="init">
    <echo message = "Starting to compile code" />
    <javac srcdir="${base_loc.java}" destdir="${build_class}">
        <classpath refid="totalClassPath"/>
    </javac>
    <echo message = "Finishing compilation of code" />
</target>

<target name="copyFolders" depends="compile_code">
    <echo message = "Starting to copy Folders" />
    <copy todir="${build_dir}/Compiled_Code" overwrite="yes">
        <fileset dir="${build_class}"/>
    </copy>
    <echo message = "Finishing copy Folders" />
</target>

<target name="createZip" depends="compile_code">
    <echo message = "Starting to create Zip" />
    <zip basedir="${build_class}"
    destfile="${zip_file_loc}/${compiled_zip_file_name}"/>
    <echo message = "Finishing creatation of Zip" />
</target>

</project>
```

Copy the source code Java files at the following location:
C:/Documentum_Source/DFC/src

Compiled classes will be formed by ANT at the following location:
C:/Documentum_Build/Compiled

dfc.jar and dfcbase.jar files are read by ANT from the following location:
C:/Program Files/Documentum/Shared

The following tasks are performed by ANT in the order shown:

+   - **Initializing** (`<target name="init">`): Build directory and compiled classes directory is created by ANT.
+   - **Compiling Code** (`<target name="compile_code" depends="init">`): Invoke javac to compile Java source code from source directory and copy the compiled classes to the compiled directory.