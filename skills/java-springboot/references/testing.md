# Testing

## Test Slice Quick Reference

| Test Type | Annotation | Use For |
|-----------|------------|---------|
| Unit | `@ExtendWith(MockitoExtension.class)` | Service logic |
| Web | `@WebMvcTest` | Controllers |
| Data | `@DataJpaTest` | Repositories |
| Integration | `@SpringBootTest` | Full flow |
| Slice | `@RestClientTest` | REST clients |

For comprehensive testing guidance, see the [java-springboot-testing](../../java-springboot-testing/SKILL.md) skill.
