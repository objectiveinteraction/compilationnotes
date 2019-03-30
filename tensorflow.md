# tensorflow compilation notes

1. the map.h in protobuf has conflicting definitions during compile time
    * explicit dereference the m_ innermap pointer
    * change m_->func() to (*m_).func
2. the crosstools clang tpl is used across all files to provide the execution environment. Changes can be made there
