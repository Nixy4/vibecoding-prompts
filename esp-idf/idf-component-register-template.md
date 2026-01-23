```cmake
set(SRC_DIRS "dir1" "dir2" "dir3")
set(INCLUDE_DIRS "dir1" "dir2" "dir3")
set(REQUIRES_ESP_IDF "idf-component1" "idf-component2" "idf-component3")
set(REQUIRES_ESP_REGISTRY "registry-component1" "registry-component2" "registry-component3")
set(REQUIRES_PROJECT "project-component1" "project-component2" "project-component3")
set(REQUIRES ${REQUIRES_ESP_IDF} ${REQUIRES_ESP_REGISTRY} ${REQUIRES_PROJECT})
set(PRIV_REQUIRES "component1" "component2" "component3")
set(EMBED_FILES "file1" "file2" "file3")
idf_component_register(
  SRC_DIRS ${SRC_DIRS}
  INCLUDE_DIRS ${INCLUDE_DIRS}
  REQUIRES ${REQUIRES}
  PRIV_REQUIRES ${PRIV_REQUIRES}
  EMBED_FILES ${EMBED_FILES}
  )
```

