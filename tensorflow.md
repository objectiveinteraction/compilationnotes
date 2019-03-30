# tensorflow compilation notes

1. the map.h in protobuf has conflicting definitions during compile time
    * explicit dereference the m_ innermap pointer
    * change m_->func() to (*m_).func
2. the crosstools clang tpl is used across all files to provide the execution environment. Changes can be made there. Specific rules for altering compilation instructions can be fed in here. Or to the generated py file in the .cache/bazel tree
4. cuda video driver updates need to be applied to the initrd or the system will use the version during bootup. The proc nvidia driver version will not display the actual version being used
5. Minimum compilation time: 6-9 hours on --local-resource 3048,.5,1.0 -- maybe should increase these values


