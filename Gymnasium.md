
- `gym.make()` để tạo môi trường. Xem các môi trường có sẵn bằng `gym.pprint_registry()`
- Có thể khởi tạo env với `seed` hoặc options khác nhau trong `reset()`.

## Wrapper

Đây là cách modify code mà không cần chỉnh sửa gì quá nhiều.
```python
import gymnasium as gym
from gymnasium.wrappers import FlattenObservation

# Start with a complex observation space
env = gym.make("CarRacing-v3")
env.observation_space.shape
(96, 96, 3)  # 96x96 RGB image

# Wrap it to flatten the observation into a 1D array
wrapped_env = FlattenObservation(env)
wrapped_env.observation_space.shape
(27648,)  # All pixels in a single array
```